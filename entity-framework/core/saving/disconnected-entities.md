---
title: Odpojené entity – EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
uid: core/saving/disconnected-entities
ms.openlocfilehash: 421531e68ac98c0553938f1c24892701f22fef3c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417595"
---
# <a name="disconnected-entities"></a><span data-ttu-id="8a8bb-102">Odpojené entity</span><span class="sxs-lookup"><span data-stu-id="8a8bb-102">Disconnected entities</span></span>

<span data-ttu-id="8a8bb-103">Instance DbContext bude automaticky sledovat entity vracené z databáze.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="8a8bb-104">Změny provedené u těchto entit budou zjištěny při volání metody SaveChanges a databáze bude aktualizována podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="8a8bb-105">Podrobnosti najdete v tématu základní informace o [uložení](basic.md) a [související data](related-data.md) .</span><span class="sxs-lookup"><span data-stu-id="8a8bb-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="8a8bb-106">Někdy se ale entity dotazují pomocí jedné instance kontextu a pak se ukládají pomocí jiné instance.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="8a8bb-107">K tomu často dochází v případě "odpojených" scénářů, jako je například webová aplikace, ve které se tyto entity odesílají do klienta, které se odešlou do klienta, změnili, odeslali zpátky na server v žádosti a pak se uložil.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="8a8bb-108">V takovém případě musí druhá instance kontextu zjistit, jestli jsou entity nové (měly by být vložené) nebo existující (by se měly aktualizovat).</span><span class="sxs-lookup"><span data-stu-id="8a8bb-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

<!-- markdownlint-disable MD028 -->
> [!TIP]
> <span data-ttu-id="8a8bb-109">[Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) tohoto článku můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-109">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) on GitHub.</span></span>

> [!TIP]
> <span data-ttu-id="8a8bb-110">EF Core může sledovat jenom jednu instanci libovolné entity s daným hodnotou primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-110">EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="8a8bb-111">Nejlepším způsobem, jak se tomuto problému vyhnout, je použití krátkodobého kontextu pro každou jednotku, aby byl kontext spuštěný, a obsahuje entity, které jsou k němu připojené, ukládají tyto entity a pak je tento kontext vyřazený a zahozený.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-111">The best way to avoid this being an issue is to use a short-lived context for each unit-of-work such that the context starts empty, has entities attached to it, saves those entities, and then the context is disposed and discarded.</span></span>
<!-- markdownlint-enable MD028 -->

## <a name="identifying-new-entities"></a><span data-ttu-id="8a8bb-112">Identifikace nových entit</span><span class="sxs-lookup"><span data-stu-id="8a8bb-112">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="8a8bb-113">Klient identifikuje nové entity.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-113">Client identifies new entities</span></span>

<span data-ttu-id="8a8bb-114">Nejjednodušší případ, který se má řešit, je, že klient informuje server, jestli je entita nová nebo existující.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-114">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="8a8bb-115">Například požadavek na vložení nové entity se liší od žádosti o aktualizaci existující entity.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-115">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="8a8bb-116">Zbývající část této části popisuje případy, kdy je potřeba určit nějaký jiný způsob, jak vkládat nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-116">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="8a8bb-117">S automaticky generovanými klíči</span><span class="sxs-lookup"><span data-stu-id="8a8bb-117">With auto-generated keys</span></span>

<span data-ttu-id="8a8bb-118">Hodnota automaticky generovaného klíče se dá často použít k určení, jestli se entita musí vložit nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-118">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="8a8bb-119">Pokud klíč nebyl nastaven (to znamená, že má stále výchozí hodnotu CLR null, nula atd.), musí být entita nová a musí vložit.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-119">If the key has not been set (that is, it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="8a8bb-120">Na druhé straně, pokud je hodnota klíče nastavená, musí se už dřív Uložit a teď je potřeba aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-120">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="8a8bb-121">Jinými slovy, pokud má klíč hodnotu, pak byla entita dotazována, odeslána klientovi a nyní se vrátila k aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-121">In other words, if the key has a value, then the entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="8a8bb-122">Pokud je typ entity znám, je snadné kontrolovat nenastavené klíče:</span><span class="sxs-lookup"><span data-stu-id="8a8bb-122">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="8a8bb-123">EF má však také vestavěný způsob, jak to provést pro libovolný typ entity a typ klíče:</span><span class="sxs-lookup"><span data-stu-id="8a8bb-123">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="8a8bb-124">Klíče jsou nastaveny, jakmile jsou entity sledovány kontextem, a to i v případě, že je entita ve stavu přidáno.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-124">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="8a8bb-125">To pomáhá při procházení grafu entit a rozhodování o tom, co dělat s každým, například při použití rozhraní TrackGraph API.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-125">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="8a8bb-126">Hodnota klíče by měla být použita pouze způsobem, který je zde zobrazen, _předtím, než_ bude provedeno volání ke sledování entity.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-126">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="8a8bb-127">S jinými klíči</span><span class="sxs-lookup"><span data-stu-id="8a8bb-127">With other keys</span></span>

<span data-ttu-id="8a8bb-128">K identifikaci nových entit je potřeba nějaký jiný mechanismus, když se klíčové hodnoty negenerují automaticky.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-128">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="8a8bb-129">Existují dva obecné přístupy:</span><span class="sxs-lookup"><span data-stu-id="8a8bb-129">There are two general approaches to this:</span></span>

* <span data-ttu-id="8a8bb-130">Dotaz na entitu</span><span class="sxs-lookup"><span data-stu-id="8a8bb-130">Query for the entity</span></span>
* <span data-ttu-id="8a8bb-131">Předat příznak z klienta</span><span class="sxs-lookup"><span data-stu-id="8a8bb-131">Pass a flag from the client</span></span>

<span data-ttu-id="8a8bb-132">Chcete-li zadat dotaz na entitu, stačí použít metodu Find:</span><span class="sxs-lookup"><span data-stu-id="8a8bb-132">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="8a8bb-133">Je nad rámec tohoto dokumentu, aby zobrazoval úplný kód pro předání příznaku z klienta.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-133">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="8a8bb-134">Ve webové aplikaci to obvykle znamená, že různé požadavky na různé akce nebo předání nějakého stavu v žádosti a jejich extrakce v řadiči.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-134">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="8a8bb-135">Ukládání jednoduchých entit</span><span class="sxs-lookup"><span data-stu-id="8a8bb-135">Saving single entities</span></span>

<span data-ttu-id="8a8bb-136">Je-li známo, zda je vyžadováno vložení nebo aktualizace, lze použít buď možnost Přidat nebo aktualizovat, a to odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8a8bb-136">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="8a8bb-137">Pokud však entita používá automaticky generované hodnoty klíčů, může být metoda aktualizace použita v obou případech:</span><span class="sxs-lookup"><span data-stu-id="8a8bb-137">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="8a8bb-138">Metoda aktualizace obvykle označí entitu pro aktualizaci, nikoli INSERT.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-138">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="8a8bb-139">Pokud však entita obsahuje automaticky generovaný klíč a nebyla nastavena žádná hodnota klíče, je entita namísto toho automaticky označena pro vložení.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-139">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="8a8bb-140">Toto chování bylo zavedeno v EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-140">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="8a8bb-141">V dřívějších verzích je vždycky nutné explicitně zvolit možnost Přidat nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-141">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="8a8bb-142">Pokud entita nepoužívá automaticky generované klíče, musí se rozhodnout, zda má být entita vložena nebo aktualizována: například:</span><span class="sxs-lookup"><span data-stu-id="8a8bb-142">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="8a8bb-143">Postup najdete tady:</span><span class="sxs-lookup"><span data-stu-id="8a8bb-143">The steps here are:</span></span>

* <span data-ttu-id="8a8bb-144">Pokud funkce Find vrátí hodnotu null, databáze již neobsahuje blog s tímto ID, proto zavolejte metodu Add, kterou označíte pro vložení.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-144">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="8a8bb-145">Pokud funkce Find vrátí entitu, pak v databázi existuje a kontext teď sleduje existující entitu.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-145">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="8a8bb-146">Pak pomocí vlastností SetValue nastavíme hodnoty všech vlastností u této entity na ty, které pocházejí z klienta.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-146">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="8a8bb-147">Volání NastavitHodnotu označí entitu, která se má aktualizovat podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-147">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="8a8bb-148">SetValues bude označovat jenom jako změněné vlastnosti, které mají jiné hodnoty jako ty ve sledované entitě.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-148">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="8a8bb-149">To znamená, že při odeslání aktualizace se aktualizují pouze sloupce, které se skutečně změnily.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-149">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="8a8bb-150">(A pokud se nic nezměnilo, nepošle se vůbec žádná aktualizace.)</span><span class="sxs-lookup"><span data-stu-id="8a8bb-150">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="8a8bb-151">Práce s grafy</span><span class="sxs-lookup"><span data-stu-id="8a8bb-151">Working with graphs</span></span>

### <a name="identity-resolution"></a><span data-ttu-id="8a8bb-152">Překlad identity</span><span class="sxs-lookup"><span data-stu-id="8a8bb-152">Identity resolution</span></span>

<span data-ttu-id="8a8bb-153">Jak je uvedeno výše, EF Core může sledovat jenom jednu instanci libovolné entity s daným hodnotou primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-153">As noted above, EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="8a8bb-154">Při práci s grafy by se graf měl ideálně vytvořit tak, že se zachová tato invariantní a kontext by měl být použit pouze pro jednu jednotku v práci.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-154">When working with graphs the graph should ideally be created such that this invariant is maintained, and the context should be used for only one unit-of-work.</span></span> <span data-ttu-id="8a8bb-155">Pokud graf obsahuje duplicity, pak bude nutné zpracovat graf před jeho odesláním do EF, aby bylo možné konsolidovat více instancí do jednoho.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-155">If the graph does contain duplicates, then it will be necessary to process the graph before sending it to EF to consolidate multiple instances into one.</span></span> <span data-ttu-id="8a8bb-156">Nemusí se jednat o triviální, kde mají instance konfliktní hodnoty a relace, takže by se měly v kanálu aplikace co nejdřív dělat konsolidace duplicit, aby se zabránilo řešení konfliktů.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-156">This may not be trivial where instances have conflicting values and relationships, so consolidating duplicates should be done as soon as possible in your application pipeline to avoid conflict resolution.</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="8a8bb-157">Všechny nové/všechny existující entity</span><span class="sxs-lookup"><span data-stu-id="8a8bb-157">All new/all existing entities</span></span>

<span data-ttu-id="8a8bb-158">Příkladem práce s grafy je vložení nebo aktualizace blogu spolu s kolekcí přidružených příspěvků.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-158">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="8a8bb-159">Pokud by měly být všechny entity v grafu vložené nebo by se měly aktualizovat všechny, pak je tento proces stejný, jak je popsáno výše u jednotlivých entit.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-159">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="8a8bb-160">Například graf blogů a příspěvků vytvořených takto:</span><span class="sxs-lookup"><span data-stu-id="8a8bb-160">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="8a8bb-161">může být vložena takto:</span><span class="sxs-lookup"><span data-stu-id="8a8bb-161">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="8a8bb-162">Volání metody Add bude označovat blog a všechny příspěvky, které mají být vloženy.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-162">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="8a8bb-163">Podobně platí, že pokud je potřeba aktualizovat všechny entity v grafu, můžete použít aktualizaci:</span><span class="sxs-lookup"><span data-stu-id="8a8bb-163">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="8a8bb-164">Blog a všechny jeho příspěvky budou označeny jako aktualizované.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-164">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="8a8bb-165">Kombinace nových a stávajících entit</span><span class="sxs-lookup"><span data-stu-id="8a8bb-165">Mix of new and existing entities</span></span>

<span data-ttu-id="8a8bb-166">U automaticky generovaných klíčů se dá aktualizace znovu použít pro vložení i aktualizace i v případě, že graf obsahuje kombinaci entit, které vyžadují vložení a ty, které vyžadují aktualizaci:</span><span class="sxs-lookup"><span data-stu-id="8a8bb-166">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="8a8bb-167">Aktualizace označí každou entitu v grafu, blogu nebo příspěvku, pokud nemá nastavenou hodnotu klíče, zatímco všechny ostatní entity jsou označené k aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-167">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="8a8bb-168">Stejně jako dřív, pokud nepoužíváte automaticky generované klíče, je možné použít dotaz a některé zpracování:</span><span class="sxs-lookup"><span data-stu-id="8a8bb-168">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="8a8bb-169">Zpracování odstranění</span><span class="sxs-lookup"><span data-stu-id="8a8bb-169">Handling deletes</span></span>

<span data-ttu-id="8a8bb-170">Odstranění může být obtížné zvládnout, protože často absence entity znamená, že by se mělo odstranit.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-170">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="8a8bb-171">Jedním ze způsobů, jak se toho vypořádat, je použít "obnovitelné odstranění" tak, aby se entita označila jako Odstraněná, a ne skutečně odstranit.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-171">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="8a8bb-172">Odstranění se pak bude shodovat s aktualizacemi.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-172">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="8a8bb-173">Obnovitelné odstranění lze implementovat pomocí [filtrů dotazů](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="8a8bb-173">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="8a8bb-174">Pro skutečný způsob odstranění je běžným vzorem použití rozšíření vzoru dotazu k provedení toho, co je v podstatě rozdíl v grafu.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-174">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff.</span></span> <span data-ttu-id="8a8bb-175">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8a8bb-175">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="8a8bb-176">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="8a8bb-176">TrackGraph</span></span>

<span data-ttu-id="8a8bb-177">Interně, přidejte, připojte a aktualizujte použít graf-prochází se stanovením pro každou entitu jako na to, jestli by měla být označená jako přidaná (pro vložení), upravená (k aktualizaci), beze změny (nedělat nic) nebo smazána (k odstranění).</span><span class="sxs-lookup"><span data-stu-id="8a8bb-177">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="8a8bb-178">Tento mechanismus se zveřejňuje prostřednictvím rozhraní TrackGraph API.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-178">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="8a8bb-179">Předpokládejme například, že když klient pošle zpátky graf entit, nastaví u každé entity příznak, který označuje, jak by se měl zpracovat.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-179">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="8a8bb-180">TrackGraph se pak dá použít ke zpracování tohoto příznaku:</span><span class="sxs-lookup"><span data-stu-id="8a8bb-180">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="8a8bb-181">Příznaky jsou zobrazeny pouze jako součást entity pro zjednodušení příkladu.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-181">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="8a8bb-182">Příznaky by byly typicky součástí DTO nebo nějakého jiného stavu zahrnutého v žádosti.</span><span class="sxs-lookup"><span data-stu-id="8a8bb-182">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
