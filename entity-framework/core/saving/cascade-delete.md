---
title: Kaskádové odstranění - EF jádro
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: 6e92b869d691d0224abf1997d9eb7ea035489c5d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417612"
---
# <a name="cascade-delete"></a><span data-ttu-id="5b7a0-102">Kaskádové odstranění</span><span class="sxs-lookup"><span data-stu-id="5b7a0-102">Cascade Delete</span></span>

<span data-ttu-id="5b7a0-103">Kaskádové odstranění se běžně používá v databázové terminologii k popisu charakteristiky, která umožňuje odstranění řádku automaticky spustit odstranění souvisejících řádků.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="5b7a0-104">Úzce související koncept, který se také vztahuje chování odstranění jádra EF, je automatické odstranění podřízené entity, když byl přerušen jeho vztah k nadřazené entitě – to to je obecně známé jako "odstranění sirotků".</span><span class="sxs-lookup"><span data-stu-id="5b7a0-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="5b7a0-105">EF Core implementuje několik různých chování odstranění a umožňuje konfiguraci chování odstranění jednotlivých relací.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="5b7a0-106">EF Core také implementuje konvence, které automaticky konfigurují užitečné výchozí chování odstranění pro každý vztah na základě [požadované relace](../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="5b7a0-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="5b7a0-107">Odstranit chování</span><span class="sxs-lookup"><span data-stu-id="5b7a0-107">Delete behaviors</span></span>

<span data-ttu-id="5b7a0-108">Odstranění chování jsou definovány v *DeleteBehavior* enumerator typu a mohou být předány *OnDelete* plynulý rozhraní API řídit, zda odstranění hlavní/nadřazené entity nebo přerušení vztahu závislé/podřízené entity by měl mít vedlejší účinek na závislé/podřízené entity.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="5b7a0-109">Existují tři akce EF může provést při odstranění hlavní/nadřazené entity nebo vztah k podřízené mu je oddělen:</span><span class="sxs-lookup"><span data-stu-id="5b7a0-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>

* <span data-ttu-id="5b7a0-110">Podřízenou/závislou osobu lze odstranit.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="5b7a0-111">Hodnoty cizího klíče dítěte lze nastavit na hodnotu null</span><span class="sxs-lookup"><span data-stu-id="5b7a0-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="5b7a0-112">Dítě zůstává nezměněno</span><span class="sxs-lookup"><span data-stu-id="5b7a0-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="5b7a0-113">Chování odstranění nakonfigurované v modelu EF Core se použije pouze v případě, že je hlavní entita odstraněna pomocí EF Core a závislé entity jsou načteny do paměti (to znamená pro sledované závislé osoby).</span><span class="sxs-lookup"><span data-stu-id="5b7a0-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (that is, for tracked dependents).</span></span> <span data-ttu-id="5b7a0-114">Odpovídající kaskádové chování musí být nastaveno v databázi, aby bylo zajištěno, že data, která nejsou sledována kontextem, mají potřebnou akci.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="5b7a0-115">Pokud k vytvoření databáze použijete EF Core, bude toto kaskádové chování nastaveno pro vás.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="5b7a0-116">Pro druhou akci výše nastavení hodnoty cizího klíče na hodnotu null není platné, pokud cizí klíč nelze stanovit hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="5b7a0-117">(Cizí klíč, který neuplatní, je ekvivalentní požadovanému vztahu.) V těchto případech EF Core sleduje, že vlastnost cizího klíče byla označena jako null, dokud SaveChanges je volána, kdy je vyvolána výjimka, protože změna nemůže být trvalé do databáze.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="5b7a0-118">To je podobné získání porušení omezení z databáze.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="5b7a0-119">Existují čtyři chování odstranění, jak je uvedeno v následujících tabulkách.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-119">There are four delete behaviors, as listed in the tables below.</span></span>

### <a name="optional-relationships"></a><span data-ttu-id="5b7a0-120">Volitelné vztahy</span><span class="sxs-lookup"><span data-stu-id="5b7a0-120">Optional relationships</span></span>

<span data-ttu-id="5b7a0-121">Pro volitelné vztahy (cizí klíč s možnou hodnotou s možnou hodnotou s platností, kterou lze ukládat za cenu null), _je_ možné uložit hodnotu cizího klíče null, což má za následek následující účinky:</span><span class="sxs-lookup"><span data-stu-id="5b7a0-121">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="5b7a0-122">Název chování</span><span class="sxs-lookup"><span data-stu-id="5b7a0-122">Behavior Name</span></span>               | <span data-ttu-id="5b7a0-123">Vliv na závislé/dítě v paměti</span><span class="sxs-lookup"><span data-stu-id="5b7a0-123">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="5b7a0-124">Vliv na závislé/podřízené v databázi</span><span class="sxs-lookup"><span data-stu-id="5b7a0-124">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="5b7a0-125">**Kaskády**</span><span class="sxs-lookup"><span data-stu-id="5b7a0-125">**Cascade**</span></span>                 | <span data-ttu-id="5b7a0-126">Entity jsou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-126">Entities are deleted</span></span>                   | <span data-ttu-id="5b7a0-127">Entity jsou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-127">Entities are deleted</span></span>                   |
| <span data-ttu-id="5b7a0-128">**ClientSetNull** (výchozí)</span><span class="sxs-lookup"><span data-stu-id="5b7a0-128">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="5b7a0-129">Vlastnosti cizího klíče jsou nastaveny na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-129">Foreign key properties are set to null</span></span> | <span data-ttu-id="5b7a0-130">Žádný</span><span class="sxs-lookup"><span data-stu-id="5b7a0-130">None</span></span>                                   |
| <span data-ttu-id="5b7a0-131">**Nastavit hodnotu Null**</span><span class="sxs-lookup"><span data-stu-id="5b7a0-131">**SetNull**</span></span>                 | <span data-ttu-id="5b7a0-132">Vlastnosti cizího klíče jsou nastaveny na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-132">Foreign key properties are set to null</span></span> | <span data-ttu-id="5b7a0-133">Vlastnosti cizího klíče jsou nastaveny na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-133">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="5b7a0-134">**Omezit**</span><span class="sxs-lookup"><span data-stu-id="5b7a0-134">**Restrict**</span></span>                | <span data-ttu-id="5b7a0-135">Žádný</span><span class="sxs-lookup"><span data-stu-id="5b7a0-135">None</span></span>                                   | <span data-ttu-id="5b7a0-136">Žádný</span><span class="sxs-lookup"><span data-stu-id="5b7a0-136">None</span></span>                                   |

### <a name="required-relationships"></a><span data-ttu-id="5b7a0-137">Požadované vztahy</span><span class="sxs-lookup"><span data-stu-id="5b7a0-137">Required relationships</span></span>

<span data-ttu-id="5b7a0-138">Pro požadované vztahy (cizí klíč, který nelze utaji) _není_ možné uložit hodnotu cizího klíče null, což má za následek následující účinky:</span><span class="sxs-lookup"><span data-stu-id="5b7a0-138">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="5b7a0-139">Název chování</span><span class="sxs-lookup"><span data-stu-id="5b7a0-139">Behavior Name</span></span>         | <span data-ttu-id="5b7a0-140">Vliv na závislé/dítě v paměti</span><span class="sxs-lookup"><span data-stu-id="5b7a0-140">Effect on dependent/child in memory</span></span> | <span data-ttu-id="5b7a0-141">Vliv na závislé/podřízené v databázi</span><span class="sxs-lookup"><span data-stu-id="5b7a0-141">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="5b7a0-142">**Kaskáda** (výchozí)</span><span class="sxs-lookup"><span data-stu-id="5b7a0-142">**Cascade** (Default)</span></span> | <span data-ttu-id="5b7a0-143">Entity jsou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-143">Entities are deleted</span></span>                | <span data-ttu-id="5b7a0-144">Entity jsou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-144">Entities are deleted</span></span>                  |
| <span data-ttu-id="5b7a0-145">**Hodnota ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="5b7a0-145">**ClientSetNull**</span></span>     | <span data-ttu-id="5b7a0-146">SaveChanges hází</span><span class="sxs-lookup"><span data-stu-id="5b7a0-146">SaveChanges throws</span></span>                  | <span data-ttu-id="5b7a0-147">Žádný</span><span class="sxs-lookup"><span data-stu-id="5b7a0-147">None</span></span>                                  |
| <span data-ttu-id="5b7a0-148">**Nastavit hodnotu Null**</span><span class="sxs-lookup"><span data-stu-id="5b7a0-148">**SetNull**</span></span>           | <span data-ttu-id="5b7a0-149">SaveChanges hází</span><span class="sxs-lookup"><span data-stu-id="5b7a0-149">SaveChanges throws</span></span>                  | <span data-ttu-id="5b7a0-150">SaveChanges hází</span><span class="sxs-lookup"><span data-stu-id="5b7a0-150">SaveChanges throws</span></span>                    |
| <span data-ttu-id="5b7a0-151">**Omezit**</span><span class="sxs-lookup"><span data-stu-id="5b7a0-151">**Restrict**</span></span>          | <span data-ttu-id="5b7a0-152">Žádný</span><span class="sxs-lookup"><span data-stu-id="5b7a0-152">None</span></span>                                | <span data-ttu-id="5b7a0-153">Žádný</span><span class="sxs-lookup"><span data-stu-id="5b7a0-153">None</span></span>                                  |

<span data-ttu-id="5b7a0-154">Ve výše uvedených tabulkách *none* může mít za následek porušení omezení.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-154">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="5b7a0-155">Například pokud objekt zabezpečení/podřízená entita je odstraněn, ale žádná akce je přijata ke změně cizího klíče závislé/podřízené, pak databáze bude pravděpodobně vyvolat SaveChanges z důvodu narušení cizí omezení.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-155">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="5b7a0-156">Na vysoké úrovni:</span><span class="sxs-lookup"><span data-stu-id="5b7a0-156">At a high level:</span></span>

* <span data-ttu-id="5b7a0-157">Pokud máte entity, které nemohou existovat bez nadřazené položky, a chcete, aby ef dbát na odstranění podřízených automaticky, pak použijte *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-157">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="5b7a0-158">Entity, které nemohou existovat bez nadřazeného objektu, obvykle využívají požadované vztahy, pro které je *výchozí hodnota.*</span><span class="sxs-lookup"><span data-stu-id="5b7a0-158">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="5b7a0-159">Pokud máte entity, které mohou nebo nemusí mít nadřazený objekt, a chcete, aby ef postarat o zrušení cizího klíče pro vás, použijte *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="5b7a0-159">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="5b7a0-160">Entity, které mohou existovat bez nadřazeného objektu, obvykle využívají volitelné vztahy, pro které je výchozí *hodnota ClientSetNull.*</span><span class="sxs-lookup"><span data-stu-id="5b7a0-160">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="5b7a0-161">Pokud chcete, aby databáze také pokusit se šířit hodnoty null na podřízené cizí klíče i v případě, že podřízená entita není načtena, použijte *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-161">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="5b7a0-162">Všimněte si však, že databáze musí podporovat toto a konfigurace databáze, jako je tento může mít za následek další omezení, která v praxi často umožňuje tuto možnost nepraktické.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-162">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="5b7a0-163">To je důvod, proč *SetNull* není výchozí.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-163">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="5b7a0-164">Pokud nechcete, aby EF Core někdy automaticky odstranil entitu nebo automaticky odstranil cizí klíč, použijte *možnost Omezit*.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-164">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="5b7a0-165">Všimněte si, že to vyžaduje, aby váš kód zachovat podřízené entity a jejich hodnoty cizího klíče v synchronizaci ručně jinak omezení výjimky budou vyvolány.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-165">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="5b7a0-166">V EF Core, na rozdíl od EF6 kaskádové efekty nedojde okamžitě, ale místo toho pouze při SaveChanges je volána.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-166">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="5b7a0-167">**Změny v EF Core 2.0:** V předchozích verzích *restrict* by způsobit volitelné cizí klíč vlastnosti v sledované závislé entity, které mají být nastaveny na hodnotu null a byl výchozí chování odstranění pro volitelné vztahy.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-167">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="5b7a0-168">V EF Core 2.0 *ClientSetNull* byla zavedena reprezentovat toto chování a stal se výchozí pro volitelné vztahy.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-168">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="5b7a0-169">Chování *Restrict* byla upravena tak, aby nikdy žádné vedlejší účinky na závislé entity.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-169">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="5b7a0-170">Příklady odstranění entity</span><span class="sxs-lookup"><span data-stu-id="5b7a0-170">Entity deletion examples</span></span>

<span data-ttu-id="5b7a0-171">Níže uvedený kód je součástí [ukázky,](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) kterou lze stáhnout a spustit.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-171">The code below is part of a [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="5b7a0-172">Ukázka ukazuje, co se stane pro každé chování odstranění pro volitelné i požadované vztahy při odstranění nadřazené entity.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-172">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="5b7a0-173">Pojďme projít každou variantu pochopit, co se děje.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-173">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="5b7a0-174">DeleteBehavior.Cascade s povinnou nebo volitelnou relace</span><span class="sxs-lookup"><span data-stu-id="5b7a0-174">DeleteBehavior.Cascade with required or optional relationship</span></span>

```console
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="5b7a0-175">Blog je označen jako Odstraněný</span><span class="sxs-lookup"><span data-stu-id="5b7a0-175">Blog is marked as Deleted</span></span>
* <span data-ttu-id="5b7a0-176">Příspěvky zpočátku zůstávají beze změny, protože kaskády se nedějí, dokud SaveChanges</span><span class="sxs-lookup"><span data-stu-id="5b7a0-176">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="5b7a0-177">SaveChanges odešle odstraní pro oba závislé osoby /podřízené příspěvky (příspěvky) a pak hlavní /nadřazený (blog)</span><span class="sxs-lookup"><span data-stu-id="5b7a0-177">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="5b7a0-178">Po uložení jsou všechny entity odpojeny, protože byly nyní odstraněny z databáze.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-178">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="5b7a0-179">DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s požadovaným vztahem</span><span class="sxs-lookup"><span data-stu-id="5b7a0-179">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="5b7a0-180">Blog je označen jako Odstraněný</span><span class="sxs-lookup"><span data-stu-id="5b7a0-180">Blog is marked as Deleted</span></span>
* <span data-ttu-id="5b7a0-181">Příspěvky zpočátku zůstávají beze změny, protože kaskády se nedějí, dokud SaveChanges</span><span class="sxs-lookup"><span data-stu-id="5b7a0-181">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="5b7a0-182">SaveChanges pokusí nastavit post FK na hodnotu null, ale to se nezdaří, protože FK není nullable</span><span class="sxs-lookup"><span data-stu-id="5b7a0-182">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="5b7a0-183">DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s volitelným vztahem</span><span class="sxs-lookup"><span data-stu-id="5b7a0-183">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="5b7a0-184">Blog je označen jako Odstraněný</span><span class="sxs-lookup"><span data-stu-id="5b7a0-184">Blog is marked as Deleted</span></span>
* <span data-ttu-id="5b7a0-185">Příspěvky zpočátku zůstávají beze změny, protože kaskády se nedějí, dokud SaveChanges</span><span class="sxs-lookup"><span data-stu-id="5b7a0-185">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="5b7a0-186">SaveChanges pokusy nastaví FK obou závislých /podřízených (příspěvky) na null před odstraněním jistiny/nadřazené (blog)</span><span class="sxs-lookup"><span data-stu-id="5b7a0-186">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="5b7a0-187">Po uložení se odstraní jistina/nadřazená položka (blog), ale závislé osoby/podřízené položky (příspěvky) jsou stále sledovány.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-187">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="5b7a0-188">Sledované závislé osoby /podřízené položky (příspěvky) mají nyní nulové hodnoty FK a jejich odkaz na odstraněný hlavní/nadřazený (blog) byl odstraněn</span><span class="sxs-lookup"><span data-stu-id="5b7a0-188">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="5b7a0-189">DeleteBehavior.Restrict s povinnou nebo volitelnou vztah</span><span class="sxs-lookup"><span data-stu-id="5b7a0-189">DeleteBehavior.Restrict with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="5b7a0-190">Blog je označen jako Odstraněný</span><span class="sxs-lookup"><span data-stu-id="5b7a0-190">Blog is marked as Deleted</span></span>
* <span data-ttu-id="5b7a0-191">Příspěvky zpočátku zůstávají beze změny, protože kaskády se nedějí, dokud SaveChanges</span><span class="sxs-lookup"><span data-stu-id="5b7a0-191">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="5b7a0-192">Vzhledem k *tomu, že funkce Omezit* říká EF, aby automaticky nenastavila hodnotu FK na hodnotu null, zůstává nenullová a saveChanges vyvolá bez uložení</span><span class="sxs-lookup"><span data-stu-id="5b7a0-192">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="5b7a0-193">Odstranit příklady osamocené</span><span class="sxs-lookup"><span data-stu-id="5b7a0-193">Delete orphans examples</span></span>

<span data-ttu-id="5b7a0-194">Níže uvedený kód je součástí [ukázky,](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) kterou lze stáhnout a spustit.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-194">The code below is part of a [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="5b7a0-195">Ukázka ukazuje, co se stane pro každé chování odstranění pro volitelné i požadované vztahy při přerušeném vztahu mezi nadřazeným/primárním objektem a jeho podřízenými/závislými položkami.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-195">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="5b7a0-196">V tomto příkladu je vztah přerušen odebráním závislé osoby/podřízené položky (příspěvky) z kolekce navigační vlastnost na hlavní nebo nadřazený (blog).</span><span class="sxs-lookup"><span data-stu-id="5b7a0-196">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="5b7a0-197">Chování je však stejné, pokud je odkaz z dependent/child na principal/parent zrušen.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-197">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="5b7a0-198">Pojďme projít každou variantu pochopit, co se děje.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-198">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="5b7a0-199">DeleteBehavior.Cascade s povinnou nebo volitelnou relace</span><span class="sxs-lookup"><span data-stu-id="5b7a0-199">DeleteBehavior.Cascade with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="5b7a0-200">Příspěvky jsou označeny jako Změněno, protože přerušení vztahu způsobilo, že FK byl označen jako null</span><span class="sxs-lookup"><span data-stu-id="5b7a0-200">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="5b7a0-201">Pokud FK nelze zrušit, skutečná hodnota se nezmění, i když je označena jako null</span><span class="sxs-lookup"><span data-stu-id="5b7a0-201">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="5b7a0-202">SaveChanges odešle odstraní pro závislé osoby /podřízené příspěvky (příspěvky)</span><span class="sxs-lookup"><span data-stu-id="5b7a0-202">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="5b7a0-203">Po uložení jsou závislé osoby/podřízené položky (příspěvky) odpojeny, protože byly nyní odstraněny z databáze</span><span class="sxs-lookup"><span data-stu-id="5b7a0-203">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="5b7a0-204">DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s požadovaným vztahem</span><span class="sxs-lookup"><span data-stu-id="5b7a0-204">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="5b7a0-205">Příspěvky jsou označeny jako Změněno, protože přerušení vztahu způsobilo, že FK byl označen jako null</span><span class="sxs-lookup"><span data-stu-id="5b7a0-205">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="5b7a0-206">Pokud FK nelze zrušit, skutečná hodnota se nezmění, i když je označena jako null</span><span class="sxs-lookup"><span data-stu-id="5b7a0-206">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="5b7a0-207">SaveChanges pokusí nastavit post FK na hodnotu null, ale to se nezdaří, protože FK není nullable</span><span class="sxs-lookup"><span data-stu-id="5b7a0-207">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="5b7a0-208">DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s volitelným vztahem</span><span class="sxs-lookup"><span data-stu-id="5b7a0-208">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="5b7a0-209">Příspěvky jsou označeny jako Změněno, protože přerušení vztahu způsobilo, že FK byl označen jako null</span><span class="sxs-lookup"><span data-stu-id="5b7a0-209">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="5b7a0-210">Pokud FK nelze zrušit, skutečná hodnota se nezmění, i když je označena jako null</span><span class="sxs-lookup"><span data-stu-id="5b7a0-210">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="5b7a0-211">SaveChanges nastaví FK obou závislých osob /podřízených (příspěvky) na hodnotu null</span><span class="sxs-lookup"><span data-stu-id="5b7a0-211">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="5b7a0-212">Po uložení mají vyživovaní/podřízené osoby (příspěvky) nyní nulové hodnoty FK a jejich odkaz na odstraněný hlavní/nadřazený (blog) byl odstraněn</span><span class="sxs-lookup"><span data-stu-id="5b7a0-212">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="5b7a0-213">DeleteBehavior.Restrict s povinnou nebo volitelnou vztah</span><span class="sxs-lookup"><span data-stu-id="5b7a0-213">DeleteBehavior.Restrict with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="5b7a0-214">Příspěvky jsou označeny jako Změněno, protože přerušení vztahu způsobilo, že FK byl označen jako null</span><span class="sxs-lookup"><span data-stu-id="5b7a0-214">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="5b7a0-215">Pokud FK nelze zrušit, skutečná hodnota se nezmění, i když je označena jako null</span><span class="sxs-lookup"><span data-stu-id="5b7a0-215">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="5b7a0-216">Vzhledem k *tomu, že funkce Omezit* říká EF, aby automaticky nenastavila hodnotu FK na hodnotu null, zůstává nenullová a saveChanges vyvolá bez uložení</span><span class="sxs-lookup"><span data-stu-id="5b7a0-216">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="5b7a0-217">Kaskádové k nesledovaným entitám</span><span class="sxs-lookup"><span data-stu-id="5b7a0-217">Cascading to untracked entities</span></span>

<span data-ttu-id="5b7a0-218">Při volání *SaveChanges*, kaskády odstranění pravidla budou použity pro všechny entity, které jsou sledovány kontextem.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-218">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="5b7a0-219">To je situace ve všech výše uvedených příkladech, což je důvod, proč sql byl generován odstranit jak hlavní / rodič (blog) a všechny závislé osoby / podřízené příspěvky):</span><span class="sxs-lookup"><span data-stu-id="5b7a0-219">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="5b7a0-220">Pokud je načten pouze objekt zabezpečení – například při dotazu pro blog bez `Include(b => b.Posts)` zahrnout také příspěvky--pak SaveChanges bude generovat pouze SQL odstranit hlavní nebo nadřazený:</span><span class="sxs-lookup"><span data-stu-id="5b7a0-220">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="5b7a0-221">Závislé položky/podřízené položky (příspěvky) budou odstraněny pouze v případě, že databáze má odpovídající kaskádové chování nakonfigurováno.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-221">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="5b7a0-222">Pokud k vytvoření databáze použijete EF, bude toto kaskádové chování nastaveno za vás.</span><span class="sxs-lookup"><span data-stu-id="5b7a0-222">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
