---
title: Odpojené entity - EF jádra
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: 0b145217d40027c4b8e4746e9c5651652a28c9eb
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2018
---
# <a name="disconnected-entities"></a><span data-ttu-id="f1db2-102">Odpojené entity</span><span class="sxs-lookup"><span data-stu-id="f1db2-102">Disconnected entities</span></span>

<span data-ttu-id="f1db2-103">DbContext instance bude automaticky sledovat entity vrácená z databáze.</span><span class="sxs-lookup"><span data-stu-id="f1db2-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="f1db2-104">Změny provedené v těchto entitách bude zjištěna, pak když je volána metoda SaveChanges a databáze bude aktualizován, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="f1db2-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="f1db2-105">V tématu [základní Uložit](basic.md) a [související Data](related-data.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="f1db2-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="f1db2-106">Ale někdy entity jsou předmětem dotazování pomocí jedné instance kontextu a pak je uložena pomocí jiné instance.</span><span class="sxs-lookup"><span data-stu-id="f1db2-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="f1db2-107">K tomu často dochází v "odpojené" scénáře, jako je webová aplikace, kde entity dotaz, může odeslat klientovi, upravit, odesílání zpět na server v požadavku a pak je uložena.</span><span class="sxs-lookup"><span data-stu-id="f1db2-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="f1db2-108">V takovém případě kontext druhou instanci musí vědět, jestli entity jsou nové (musí být zařazeny) nebo existující (by měly být aktualizovány).</span><span class="sxs-lookup"><span data-stu-id="f1db2-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="f1db2-109">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="f1db2-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

> [!TIP]
> <span data-ttu-id="f1db2-110">Základní EF můžete sledovat pouze jednu instanci entitu s danou hodnotu primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="f1db2-110">EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="f1db2-111">Nejlepší způsob, jak se vyhnout této probíhá, kterou problém použít krátkodobou kontext pro každou jednotku pracovní tak, aby kontext spuštění prázdný, obsahuje entity připojený, uloží tyto entity a kontext zrušen a zahodí.</span><span class="sxs-lookup"><span data-stu-id="f1db2-111">The best way to avoid this being an issue is to use a short-lived context for each unit-of-work such that the context starts empty, has entities attached to it, saves those entities, and then the context is disposed and discarded.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="f1db2-112">Identifikace nové entity</span><span class="sxs-lookup"><span data-stu-id="f1db2-112">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="f1db2-113">Klient určí nové entity</span><span class="sxs-lookup"><span data-stu-id="f1db2-113">Client identifies new entities</span></span>

<span data-ttu-id="f1db2-114">Nejjednodušším případě řešit je, když klient informuje server, jestli entity jsou nové nebo existující.</span><span class="sxs-lookup"><span data-stu-id="f1db2-114">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="f1db2-115">Například často žádost o vložení nové entity se liší od požadavku aktualizace stávající entity.</span><span class="sxs-lookup"><span data-stu-id="f1db2-115">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="f1db2-116">Zbytek této části popisuje případy kde je potřeba určit jiným způsobem, jestli se má vložit nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="f1db2-116">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="f1db2-117">Automaticky generovaného klíče</span><span class="sxs-lookup"><span data-stu-id="f1db2-117">With auto-generated keys</span></span>

<span data-ttu-id="f1db2-118">Hodnota automaticky generovaného klíče můžete často používá k určení, jestli entity je nutné vložit nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="f1db2-118">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="f1db2-119">Pokud klíč nebylo nebyl nastavený, (tj. stále má CLR výchozí hodnotu null, nula, atd.), pak musí být nové entity a potřebuje vkládání.</span><span class="sxs-lookup"><span data-stu-id="f1db2-119">If the key has not been set (i.e. it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="f1db2-120">Na druhé straně-li nastavena hodnota klíče, pak se musí již byly dříve uloženy a nyní je nutné aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="f1db2-120">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="f1db2-121">Jinými slovy Pokud klíč má hodnotu, pak entity byla dotazována může odeslat klientovi a má teď vraťte aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="f1db2-121">In other words, if the key has a value, then entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="f1db2-122">Je snadné vyhledávat nenastavené klíč, když je známý typ entity:</span><span class="sxs-lookup"><span data-stu-id="f1db2-122">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="f1db2-123">EF má však také předdefinovaný způsob, jak to udělat pro libovolného typu entity a typ klíče:</span><span class="sxs-lookup"><span data-stu-id="f1db2-123">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="f1db2-124">Klíče jsou nastaveny také entity jsou sledovány objektem kontext, i v případě, že tato entita je ve stavu Added.</span><span class="sxs-lookup"><span data-stu-id="f1db2-124">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="f1db2-125">To pomáhá při procházení graf entity a rozhodování, co dělat s každou, například, jako při použití rozhraní API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="f1db2-125">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="f1db2-126">Hodnota klíče by měl použít pouze ve způsobu, jakým jsou tady uvedené _před_ žádné Přišla žádost o sledování entity.</span><span class="sxs-lookup"><span data-stu-id="f1db2-126">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="f1db2-127">Pomocí jiných klíčů</span><span class="sxs-lookup"><span data-stu-id="f1db2-127">With other keys</span></span>

<span data-ttu-id="f1db2-128">K identifikaci nové entity, když nejsou automaticky generované hodnoty klíče je nutné jinému kontrolnímu mechanismu.</span><span class="sxs-lookup"><span data-stu-id="f1db2-128">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="f1db2-129">Existují dvě obecné přístupy k tomuto:</span><span class="sxs-lookup"><span data-stu-id="f1db2-129">There are two general approaches to this:</span></span>
 * <span data-ttu-id="f1db2-130">Dotaz pro entitu</span><span class="sxs-lookup"><span data-stu-id="f1db2-130">Query for the entity</span></span>
 * <span data-ttu-id="f1db2-131">Předat příznak z klienta</span><span class="sxs-lookup"><span data-stu-id="f1db2-131">Pass a flag from the client</span></span>

<span data-ttu-id="f1db2-132">Dotaz pro entitu, stačí použijte metodu najít:</span><span class="sxs-lookup"><span data-stu-id="f1db2-132">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="f1db2-133">Je nad rámec tohoto dokumentu zobrazit úplného kódu pro předávání příznak z klienta.</span><span class="sxs-lookup"><span data-stu-id="f1db2-133">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="f1db2-134">Ve webové aplikaci obvykle to znamená provedení různých žádostí pro různé akce, nebo předávání některých stavu v požadavku a extrahování v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="f1db2-134">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="f1db2-135">Ukládání jedné entity</span><span class="sxs-lookup"><span data-stu-id="f1db2-135">Saving single entities</span></span>

<span data-ttu-id="f1db2-136">Pokud je známo, zda je potřeba příkazu insert nebo update a potom přidat nebo aktualizovat slouží odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f1db2-136">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="f1db2-137">Ale entita používá automaticky generovaný hodnoty klíče, pak metodu aktualizace lze nastavit pro oba případy:</span><span class="sxs-lookup"><span data-stu-id="f1db2-137">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="f1db2-138">Metoda aktualizace označí normálně entity pro aktualizaci, není insert.</span><span class="sxs-lookup"><span data-stu-id="f1db2-138">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="f1db2-139">Ale pokud entita, která má automaticky generovaný klíč a žádná hodnota klíče je nastavená entity se místo toho automaticky označeny pro vložení.</span><span class="sxs-lookup"><span data-stu-id="f1db2-139">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="f1db2-140">Toto chování byla zavedena v EF základní 2.0.</span><span class="sxs-lookup"><span data-stu-id="f1db2-140">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="f1db2-141">Pro starší verze je vždy nutné explicitně vyberte Přidat nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="f1db2-141">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="f1db2-142">Pokud entita není pomocí automaticky generovaného klíče, pak aplikace musíte rozhodnout, zda by měla být vložit nebo aktualizovat entitu: Příklad:</span><span class="sxs-lookup"><span data-stu-id="f1db2-142">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="f1db2-143">Postup v tomto poli se:</span><span class="sxs-lookup"><span data-stu-id="f1db2-143">The steps here are:</span></span>
* <span data-ttu-id="f1db2-144">Pokud hledání vrátí hodnotu null, pak databázi už neobsahuje blog s tímto ID, se nazývá přidat ji označte pro vložení.</span><span class="sxs-lookup"><span data-stu-id="f1db2-144">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="f1db2-145">Pokud hledání vrací entity, pak existuje v databázi a kontext je nyní sledování stávající entity</span><span class="sxs-lookup"><span data-stu-id="f1db2-145">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="f1db2-146">Potom používáme SetValues nastavit hodnoty pro všechny vlastnosti pro tuto entitu na ty, které pocházejí z klienta.</span><span class="sxs-lookup"><span data-stu-id="f1db2-146">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="f1db2-147">Volání SetValues označíte entity, která má být aktualizována podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="f1db2-147">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="f1db2-148">SetValues pouze označíte jako upravená vlastnosti, které mají různé hodnoty na hodnoty v sledovaných entity.</span><span class="sxs-lookup"><span data-stu-id="f1db2-148">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="f1db2-149">To znamená, pokud jsou odesílány, je aktualizován pouze sloupce, které se změnily ve skutečnosti.</span><span class="sxs-lookup"><span data-stu-id="f1db2-149">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="f1db2-150">(A pokud se nic změnila, pak žádná aktualizace se budou odesílat vůbec.)</span><span class="sxs-lookup"><span data-stu-id="f1db2-150">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="f1db2-151">Práce s grafy</span><span class="sxs-lookup"><span data-stu-id="f1db2-151">Working with graphs</span></span>

### <a name="identity-resolution"></a><span data-ttu-id="f1db2-152">Řešení identity</span><span class="sxs-lookup"><span data-stu-id="f1db2-152">Identity resolution</span></span>

<span data-ttu-id="f1db2-153">Jak jsme uvedli výše, EF základní můžete sledovat pouze jednu instanci entitu s danou hodnotu primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="f1db2-153">As noted above, EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="f1db2-154">Při práci s grafy grafu by v ideálním případě se vytvořit tak, aby tento neutrální zachovaný a kontext se mají použít pro pouze jednu jednotku pracovní.</span><span class="sxs-lookup"><span data-stu-id="f1db2-154">When working with graphs the graph should ideally be created such that this invariant is maintained, and the context should be used for only one unit-of-work.</span></span> <span data-ttu-id="f1db2-155">Pokud graf obsahovat duplikáty, pak bude potřeba zpracovat grafu před odesláním do EF pro konsolidaci víc instancí do jednoho.</span><span class="sxs-lookup"><span data-stu-id="f1db2-155">If the graph does contain duplicates, then it will be necessary to process the graph before sending it to EF to consolidate multiple instances into one.</span></span> <span data-ttu-id="f1db2-156">Toto nemusí být trivial kde instance mají konfliktní hodnoty a vztahy, takže sloučením duplikáty by mělo být provedeno co nejdříve v kanálu aplikace předejdete řešení konfliktů.</span><span class="sxs-lookup"><span data-stu-id="f1db2-156">This may not be trivial where instances have conflicting values and relationships, so consolidating duplicates should be done as soon as possible in your application pipeline to avoid conflict resolution.</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="f1db2-157">Všechny nové nebo žádná existující entity</span><span class="sxs-lookup"><span data-stu-id="f1db2-157">All new/all existing entities</span></span>

<span data-ttu-id="f1db2-158">Příkladem práci s grafy je vložení nebo aktualizace společně s jeho kolekce přidružené příspěvky blogu.</span><span class="sxs-lookup"><span data-stu-id="f1db2-158">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="f1db2-159">Pokud má být vložen všechny entity v grafu nebo by měly být aktualizovány všechny, pak proces je stejné, jak je popsáno výše pro jedné entity.</span><span class="sxs-lookup"><span data-stu-id="f1db2-159">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="f1db2-160">Například graf blogy a příspěvcích vytvořit takto:</span><span class="sxs-lookup"><span data-stu-id="f1db2-160">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="f1db2-161">můžete vložit takto:</span><span class="sxs-lookup"><span data-stu-id="f1db2-161">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="f1db2-162">Volání přidat označíte blogu a v příspěvcích má být vložen.</span><span class="sxs-lookup"><span data-stu-id="f1db2-162">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="f1db2-163">Podobně pokud všechny entity v grafu je nutné aktualizovat, pak aktualizace lze použít:</span><span class="sxs-lookup"><span data-stu-id="f1db2-163">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="f1db2-164">Blog a všechny jeho příspěvky budou označeny aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="f1db2-164">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="f1db2-165">Kombinace nových nebo stávajících entit</span><span class="sxs-lookup"><span data-stu-id="f1db2-165">Mix of new and existing entities</span></span>

<span data-ttu-id="f1db2-166">Automaticky generovaného klíče aktualizace můžete znovu použít pro vložení a aktualizace, i v případě, že graf obsahuje kombinaci entity, které vyžadují vkládání a ty, které vyžadují aktualizaci:</span><span class="sxs-lookup"><span data-stu-id="f1db2-166">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="f1db2-167">Aktualizace označí všechny entity v grafu, blogu nebo post pro vložení, pokud nemá sadu hodnotu klíče, zatímco ostatní entity jsou označena k aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="f1db2-167">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="f1db2-168">Jako dříve, pokud nepoužíváte automaticky generovaného klíče dotazu a některé zpracování lze použít:</span><span class="sxs-lookup"><span data-stu-id="f1db2-168">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="f1db2-169">Zpracování odstranění</span><span class="sxs-lookup"><span data-stu-id="f1db2-169">Handling deletes</span></span>

<span data-ttu-id="f1db2-170">Odstranění může být složité zpracování od často znamená absenci entitu, měla by být odstraněna.</span><span class="sxs-lookup"><span data-stu-id="f1db2-170">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="f1db2-171">Jedním ze způsobů, jak nakládat s to je použití "obnovitelného odstranění" tak, že entita je označena k odstranění místo ve skutečnosti odstraňuje.</span><span class="sxs-lookup"><span data-stu-id="f1db2-171">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="f1db2-172">Odstraní se pak stane stejná jako aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f1db2-172">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="f1db2-173">Obnovitelného odstranění můžou se implementovat v pomocí [dotaz filtry](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="f1db2-173">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="f1db2-174">Pro odstranění hodnotu true má pomocí rozšíření dotazu vzoru provádět, co je v podstatě graf rozdíl běžný vzor Příklad:</span><span class="sxs-lookup"><span data-stu-id="f1db2-174">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="f1db2-175">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="f1db2-175">TrackGraph</span></span>

<span data-ttu-id="f1db2-176">Interně, přidat, připojit a aktualizace pomocí graf traversal zjištění vytvářeny pro každou entitu, jestli ho by měl být označen jako Added (Chcete-li vložit) upravené (k aktualizaci) Unchanged (nedělat nic), nebo odstraněné (Chcete-li odstranit).</span><span class="sxs-lookup"><span data-stu-id="f1db2-176">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="f1db2-177">Tento mechanismus je vystaven prostřednictvím rozhraní API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="f1db2-177">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="f1db2-178">Například předpokládejme, že když klient odešle zpět graf entit nastaví některé příznak u každé entity, která určuje, jak by měly zpracovávat.</span><span class="sxs-lookup"><span data-stu-id="f1db2-178">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="f1db2-179">TrackGraph pak umožňuje zpracovat tento příznak:</span><span class="sxs-lookup"><span data-stu-id="f1db2-179">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="f1db2-180">V příznacích se zobrazují pouze v rámci entity, pro jednoduchost v příkladu.</span><span class="sxs-lookup"><span data-stu-id="f1db2-180">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="f1db2-181">Obvykle příznaků by být součástí DTO nebo jiný stav součástí požadavku.</span><span class="sxs-lookup"><span data-stu-id="f1db2-181">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
