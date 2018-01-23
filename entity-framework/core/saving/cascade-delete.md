---
title: "Odstranění – základní EF v kaskádě"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: e1cb194d7c7472af59eb44fe2a084fa16c40c186
ms.sourcegitcommit: 3b21a7fdeddc7b3c70d9b7777b72bef61f59216c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
# <a name="cascade-delete"></a><span data-ttu-id="a44c1-102">Kaskádové odstranění</span><span class="sxs-lookup"><span data-stu-id="a44c1-102">Cascade Delete</span></span>

<span data-ttu-id="a44c1-103">Kaskádové odstranění se běžně používá v terminologii databáze k popisu vlastnosti, který umožňuje odstranění řádku automaticky spouštět odstranění související řádky.</span><span class="sxs-lookup"><span data-stu-id="a44c1-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="a44c1-104">Úzce související koncept také předmětem EF základní odstranit chování je automatické odstranění podřízené entity, když se nejedná o relaci k nadřazenému má porušeno – tento i obvykle označuje jako "odstranění osamocené položky".</span><span class="sxs-lookup"><span data-stu-id="a44c1-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this i commonly known as "deleting orphans".</span></span>

<span data-ttu-id="a44c1-105">Základní EF implementuje několik různých odstranit chování a umožňuje konfigurovat chování odstranění jednotlivých relací.</span><span class="sxs-lookup"><span data-stu-id="a44c1-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="a44c1-106">Základní EF také implementuje konvence, který automaticky nakonfiguruje užitečné výchozí chování delete pro každý vztah podle [requiredness relace] (../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="a44c1-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship] (../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="a44c1-107">Odstranit chování</span><span class="sxs-lookup"><span data-stu-id="a44c1-107">Delete behaviors</span></span>
<span data-ttu-id="a44c1-108">Odstranit chování jsou definovány v *DeleteBehavior* enumerátor typu a se dá předat do *OnDelete* rozhraní fluent API na ovládací prvek zda odstranění hlavní/nadřazená entita nebo severing z vztah k závislé nebo podřízených entit by měl mít vedlejším účinkem závislé nebo podřízených entit.</span><span class="sxs-lookup"><span data-stu-id="a44c1-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="a44c1-109">Existují tři akce, kterou EF můžete provést, pokud je odstraněný hlavní/nadřazená entita, nebo je porušeno relace k podřízené:</span><span class="sxs-lookup"><span data-stu-id="a44c1-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="a44c1-110">Podřízené/závislé mohl být odstraněn.</span><span class="sxs-lookup"><span data-stu-id="a44c1-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="a44c1-111">Cizí klíče hodnoty dítěte lze nastavit na hodnotu null</span><span class="sxs-lookup"><span data-stu-id="a44c1-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="a44c1-112">Podřízená zůstává beze změny</span><span class="sxs-lookup"><span data-stu-id="a44c1-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="a44c1-113">Odstranění chování nakonfigurovaný v základní EF modelu je použít jenom v případě hlavní entity se odstraní pomocí EF jádra a závislé entity jsou načtena do paměti (tj. pro položky závislé na sledovaných).</span><span class="sxs-lookup"><span data-stu-id="a44c1-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (i.e. for tracked dependents).</span></span> <span data-ttu-id="a44c1-114">Odpovídající chování cascade musí být, že má instalační program v databázi, aby data, která není sledována kontextu potřebné akce použitá.</span><span class="sxs-lookup"><span data-stu-id="a44c1-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="a44c1-115">Pokud používáte základní EF k vytvoření databáze, bude toto chování cascade instalace pro vás.</span><span class="sxs-lookup"><span data-stu-id="a44c1-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="a44c1-116">Druhou akci výše uvedených nastavení hodnoty cizího klíče na hodnotu null není platný, pokud cizí klíč není null.</span><span class="sxs-lookup"><span data-stu-id="a44c1-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="a44c1-117">(Použití hodnot Null cizí klíč, který je ekvivalentní požadovaná relace.) V těchto případech EF základní sleduje, zda vlastností cizího klíče byl označen jako hodnotu null. dokud se nazývá SaveChanges, po kterých je vyvolána výjimka, protože změna nelze nastavit jako trvalý, do databáze.</span><span class="sxs-lookup"><span data-stu-id="a44c1-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="a44c1-118">Toto je podobná získávání porušení omezení z databáze.</span><span class="sxs-lookup"><span data-stu-id="a44c1-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="a44c1-119">Existují čtyři odstranit chování, jak je uvedené v následujících tabulkách.</span><span class="sxs-lookup"><span data-stu-id="a44c1-119">There are four delete behaviors, as listed in the tables below.</span></span> <span data-ttu-id="a44c1-120">Pro volitelné relace (s možnou hodnotou Null cizí klíč) je _je_ možné ukládat cizího klíče hodnotu null, což vede k tyto důsledky:</span><span class="sxs-lookup"><span data-stu-id="a44c1-120">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="a44c1-121">Název chování</span><span class="sxs-lookup"><span data-stu-id="a44c1-121">Behavior Name</span></span> | <span data-ttu-id="a44c1-122">Vliv na závislé a podřízené v paměti</span><span class="sxs-lookup"><span data-stu-id="a44c1-122">Effect on dependent/child in memory</span></span> | <span data-ttu-id="a44c1-123">Vliv na závislé a podřízené databáze</span><span class="sxs-lookup"><span data-stu-id="a44c1-123">Effect on dependent/child in database</span></span>
|-|-|-
| <span data-ttu-id="a44c1-124">**Cascade**</span><span class="sxs-lookup"><span data-stu-id="a44c1-124">**Cascade**</span></span> | <span data-ttu-id="a44c1-125">Entity se odstraní.</span><span class="sxs-lookup"><span data-stu-id="a44c1-125">Entities are deleted</span></span> | <span data-ttu-id="a44c1-126">Entity se odstraní.</span><span class="sxs-lookup"><span data-stu-id="a44c1-126">Entities are deleted</span></span>
| <span data-ttu-id="a44c1-127">**ClientSetNull** (výchozí)</span><span class="sxs-lookup"><span data-stu-id="a44c1-127">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="a44c1-128">Vlastnosti cizího klíče jsou nastaveny na hodnotu null</span><span class="sxs-lookup"><span data-stu-id="a44c1-128">Foreign key properties are set to null</span></span> | <span data-ttu-id="a44c1-129">Žádné</span><span class="sxs-lookup"><span data-stu-id="a44c1-129">None</span></span>
| <span data-ttu-id="a44c1-130">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="a44c1-130">**SetNull**</span></span> | <span data-ttu-id="a44c1-131">Vlastnosti cizího klíče jsou nastaveny na hodnotu null</span><span class="sxs-lookup"><span data-stu-id="a44c1-131">Foreign key properties are set to null</span></span> | <span data-ttu-id="a44c1-132">Vlastnosti cizího klíče jsou nastaveny na hodnotu null</span><span class="sxs-lookup"><span data-stu-id="a44c1-132">Foreign key properties are set to null</span></span>
| <span data-ttu-id="a44c1-133">**Omezení**</span><span class="sxs-lookup"><span data-stu-id="a44c1-133">**Restrict**</span></span> | <span data-ttu-id="a44c1-134">Žádné</span><span class="sxs-lookup"><span data-stu-id="a44c1-134">None</span></span> | <span data-ttu-id="a44c1-135">Žádné</span><span class="sxs-lookup"><span data-stu-id="a44c1-135">None</span></span>

<span data-ttu-id="a44c1-136">Pro požadované relace (použití hodnot Null cizí klíč) je _není_ možné ukládat cizího klíče hodnotu null, což vede k tyto důsledky:</span><span class="sxs-lookup"><span data-stu-id="a44c1-136">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="a44c1-137">Název chování</span><span class="sxs-lookup"><span data-stu-id="a44c1-137">Behavior Name</span></span> | <span data-ttu-id="a44c1-138">Vliv na závislé a podřízené v paměti</span><span class="sxs-lookup"><span data-stu-id="a44c1-138">Effect on dependent/child in memory</span></span> | <span data-ttu-id="a44c1-139">Vliv na závislé a podřízené databáze</span><span class="sxs-lookup"><span data-stu-id="a44c1-139">Effect on dependent/child in database</span></span>
|-|-|-
| <span data-ttu-id="a44c1-140">**CASCADE** (výchozí)</span><span class="sxs-lookup"><span data-stu-id="a44c1-140">**Cascade** (Default)</span></span> | <span data-ttu-id="a44c1-141">Entity se odstraní.</span><span class="sxs-lookup"><span data-stu-id="a44c1-141">Entities are deleted</span></span> | <span data-ttu-id="a44c1-142">Entity se odstraní.</span><span class="sxs-lookup"><span data-stu-id="a44c1-142">Entities are deleted</span></span>
| <span data-ttu-id="a44c1-143">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="a44c1-143">**ClientSetNull**</span></span> | <span data-ttu-id="a44c1-144">Vyvolá SaveChanges</span><span class="sxs-lookup"><span data-stu-id="a44c1-144">SaveChanges throws</span></span> | <span data-ttu-id="a44c1-145">Žádné</span><span class="sxs-lookup"><span data-stu-id="a44c1-145">None</span></span>
| <span data-ttu-id="a44c1-146">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="a44c1-146">**SetNull**</span></span> | <span data-ttu-id="a44c1-147">Vyvolá SaveChanges</span><span class="sxs-lookup"><span data-stu-id="a44c1-147">SaveChanges throws</span></span> | <span data-ttu-id="a44c1-148">Vyvolá SaveChanges</span><span class="sxs-lookup"><span data-stu-id="a44c1-148">SaveChanges throws</span></span>
| <span data-ttu-id="a44c1-149">**Omezení**</span><span class="sxs-lookup"><span data-stu-id="a44c1-149">**Restrict**</span></span> | <span data-ttu-id="a44c1-150">Žádné</span><span class="sxs-lookup"><span data-stu-id="a44c1-150">None</span></span> | <span data-ttu-id="a44c1-151">Žádné</span><span class="sxs-lookup"><span data-stu-id="a44c1-151">None</span></span>

<span data-ttu-id="a44c1-152">V tabulkách výš *žádné* může mít za následek porušení omezení.</span><span class="sxs-lookup"><span data-stu-id="a44c1-152">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="a44c1-153">Například pokud je odstraněn objekt nebo podřízený entity, ale chcete-li změnit cizí klíč závislé a podřízené nebyla provedena žádná akce, poté databázi bude pravděpodobně vyvolat na SaveChanges z důvodu narušení omezení pro cizí.</span><span class="sxs-lookup"><span data-stu-id="a44c1-153">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="a44c1-154">Na vysoké úrovni:</span><span class="sxs-lookup"><span data-stu-id="a44c1-154">At a high level:</span></span>
* <span data-ttu-id="a44c1-155">Pokud máte entit, které nemůže existovat bez nadřazené a které EF abyste dbali pro odstranění podřízené objekty automaticky, a potom použijte *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="a44c1-155">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="a44c1-156">Entity, která nemůže existovat bez nadřazený obvykle provést použít požadované vztahy, pro kterou *Cascade* je výchozí.</span><span class="sxs-lookup"><span data-stu-id="a44c1-156">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="a44c1-157">Pokud máte entit, které může nebo nemusí mít nadřazenou položku, a které EF, která se postará o nulling out cizí klíč pro vás, a potom použijte *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="a44c1-157">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="a44c1-158">Entit, které může existovat bez nadřazený obvykle provést pomocí volitelné vztahy, pro kterou *ClientSetNull* je výchozí.</span><span class="sxs-lookup"><span data-stu-id="a44c1-158">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="a44c1-159">Pokud chcete databázi pokusit také potřebný k šíření hodnoty null cizí klíče podřízené i když není načtený podřízené entity, pak použijte *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="a44c1-159">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="a44c1-160">Ale Upozorňujeme, že databáze musí podporovat to a konfigurace databáze jako to může mít za následek další omezení, která v praxi často nepraktické tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="a44c1-160">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="a44c1-161">To je důvod, proč *SetNull* není výchozí.</span><span class="sxs-lookup"><span data-stu-id="a44c1-161">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="a44c1-162">Pokud nechcete, aby základní EF někdy automaticky odstranění entity nebo hodnota null se cizí klíč automaticky a pak použít *omezit*.</span><span class="sxs-lookup"><span data-stu-id="a44c1-162">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="a44c1-163">Všimněte si, že to vyžaduje, aby váš kód synchronizaci podřízených entit a jejich hodnoty cizího klíče ručně jinak omezení výjimky bude vyvolána.</span><span class="sxs-lookup"><span data-stu-id="a44c1-163">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="a44c1-164">V EF Core, na rozdíl od EF6 kaskádových důsledky není dojít okamžitě, ale místo toho pouze když je volána metoda SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="a44c1-164">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="a44c1-165">**Změny v EF základní 2.0:** v předchozích verzích *omezit* by způsobilo volitelné vlastnosti cizího klíče v sledovaných závislé entity na hodnotu null a byla odstranit výchozí chování pro volitelné relace.</span><span class="sxs-lookup"><span data-stu-id="a44c1-165">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="a44c1-166">V EF základní 2.0 *ClientSetNull* bylo zavedeno představují daná chování a aktivovala na výchozí hodnoty pro volitelné vztahy.</span><span class="sxs-lookup"><span data-stu-id="a44c1-166">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="a44c1-167">Chování *omezit* byla upravena nikdy mít žádné vedlejší účinky na závislé entity.</span><span class="sxs-lookup"><span data-stu-id="a44c1-167">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="a44c1-168">Příklady odstranění entity</span><span class="sxs-lookup"><span data-stu-id="a44c1-168">Entity deletion examples</span></span>

<span data-ttu-id="a44c1-169">Následující kód je součástí [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , je možné stáhnout a spustit.</span><span class="sxs-lookup"><span data-stu-id="a44c1-169">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="a44c1-170">Ukázka zobrazuje, co se stane pro každý odstranit chování pro volitelné a požadované relace při odstranění nadřazená entita.</span><span class="sxs-lookup"><span data-stu-id="a44c1-170">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="a44c1-171">Projděme každý variace pochopit, co se děje.</span><span class="sxs-lookup"><span data-stu-id="a44c1-171">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="a44c1-172">DeleteBehavior.Cascade s požadované nebo volitelné relace</span><span class="sxs-lookup"><span data-stu-id="a44c1-172">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="a44c1-173">Blog je označena jako odstraněné</span><span class="sxs-lookup"><span data-stu-id="a44c1-173">Blog is marked as Deleted</span></span>
* <span data-ttu-id="a44c1-174">Příspěvky původně zůstat Unchanged, protože není dojít cascades, dokud SaveChanges</span><span class="sxs-lookup"><span data-stu-id="a44c1-174">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="a44c1-175">SaveChanges odešle odstranění závislé objekty nebo podřízených prvků (příspěvky) a pak hlavní nebo nadřízené (blog)</span><span class="sxs-lookup"><span data-stu-id="a44c1-175">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="a44c1-176">Po uložení všechny entity jsou odpojené od teď z databáze byla odstraněna</span><span class="sxs-lookup"><span data-stu-id="a44c1-176">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="a44c1-177">DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s požadovaná relace</span><span class="sxs-lookup"><span data-stu-id="a44c1-177">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="a44c1-178">Blog je označena jako odstraněné</span><span class="sxs-lookup"><span data-stu-id="a44c1-178">Blog is marked as Deleted</span></span>
* <span data-ttu-id="a44c1-179">Příspěvky původně zůstat Unchanged, protože není dojít cascades, dokud SaveChanges</span><span class="sxs-lookup"><span data-stu-id="a44c1-179">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="a44c1-180">SaveChanges, pokusí se nastavit post cizího klíče na hodnotu null, ale to nezdaří, protože není Cizíklíč s možnou hodnotou Null</span><span class="sxs-lookup"><span data-stu-id="a44c1-180">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="a44c1-181">DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s volitelný vztah</span><span class="sxs-lookup"><span data-stu-id="a44c1-181">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="a44c1-182">Blog je označena jako odstraněné</span><span class="sxs-lookup"><span data-stu-id="a44c1-182">Blog is marked as Deleted</span></span>
* <span data-ttu-id="a44c1-183">Příspěvky původně zůstat Unchanged, protože není dojít cascades, dokud SaveChanges</span><span class="sxs-lookup"><span data-stu-id="a44c1-183">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="a44c1-184">SaveChanges pokusy nastaví Cizíklíč obě položky závislé na nebo podřízených prvků (příspěvky) na hodnotu null. před odstraněním hlavní nebo nadřízené (blog)</span><span class="sxs-lookup"><span data-stu-id="a44c1-184">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="a44c1-185">Po uložení se odstraní objekt zabezpečení nebo nadřízené (blog), ale stále sledovány závislé objekty nebo podřízené objekty (příspěvky)</span><span class="sxs-lookup"><span data-stu-id="a44c1-185">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="a44c1-186">Sledovaných závislé objekty nebo podřízené objekty (příspěvky) teď mít hodnotu null. hodnoty cizího klíče a odebrala se jejich odkaz na odstraněného objektu zabezpečení nebo nadřízené (blog)</span><span class="sxs-lookup"><span data-stu-id="a44c1-186">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="a44c1-187">DeleteBehavior.Restrict s požadované nebo volitelné relace</span><span class="sxs-lookup"><span data-stu-id="a44c1-187">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="a44c1-188">Blog je označena jako odstraněné</span><span class="sxs-lookup"><span data-stu-id="a44c1-188">Blog is marked as Deleted</span></span>
* <span data-ttu-id="a44c1-189">Příspěvky původně zůstat Unchanged, protože není dojít cascades, dokud SaveChanges</span><span class="sxs-lookup"><span data-stu-id="a44c1-189">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="a44c1-190">Vzhledem k tomu *omezit* informuje EF automaticky nastavit cizího klíče na hodnotu null, zůstane jinou hodnotu než null a SaveChanges vyvolá bez uložení</span><span class="sxs-lookup"><span data-stu-id="a44c1-190">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="a44c1-191">Odstranit příklady osamocené položky</span><span class="sxs-lookup"><span data-stu-id="a44c1-191">Delete orphans examples</span></span>

<span data-ttu-id="a44c1-192">Následující kód je součástí [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) který lze stáhnout spustit.</span><span class="sxs-lookup"><span data-stu-id="a44c1-192">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="a44c1-193">Ukázka zobrazuje, co se stane pro každý odstranit chování pro volitelné a požadované relace při je vztah mezi nadřazenou nebo objekt zabezpečení a jeho podřízených objektů/dependents porušeno.</span><span class="sxs-lookup"><span data-stu-id="a44c1-193">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="a44c1-194">V tomto příkladu je relace porušeno odebráním závislosti nebo podřízené objekty (příspěvky) z navigační vlastnosti kolekce na objekt zabezpečení nebo nadřízené (blog).</span><span class="sxs-lookup"><span data-stu-id="a44c1-194">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="a44c1-195">Chování je však stejný Pokud ze závislých a podřízené odkaz na objekt zabezpečení nebo nadřízené je místo toho vynulovanými limitu.</span><span class="sxs-lookup"><span data-stu-id="a44c1-195">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="a44c1-196">Projděme každý variace pochopit, co se děje.</span><span class="sxs-lookup"><span data-stu-id="a44c1-196">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="a44c1-197">DeleteBehavior.Cascade s požadované nebo volitelné relace</span><span class="sxs-lookup"><span data-stu-id="a44c1-197">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="a44c1-198">Příspěvky jsou označeny jako změněné, protože severing relace způsobilo Cizíklíč byla označena jako hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="a44c1-198">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="a44c1-199">Pokud Cizíklíč není s možnou hodnotou Null, pak se skutečnou hodnotou nedojde ke změně i když je označen jako hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="a44c1-199">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="a44c1-200">SaveChanges odešle odstranění pro závislé objekty (příspěvky) podřízené objekty</span><span class="sxs-lookup"><span data-stu-id="a44c1-200">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="a44c1-201">Po uložení položky závislé na nebo podřízené objekty (příspěvky) jsou odpojené od teď z databáze byla odstraněna</span><span class="sxs-lookup"><span data-stu-id="a44c1-201">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="a44c1-202">DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s požadovaná relace</span><span class="sxs-lookup"><span data-stu-id="a44c1-202">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="a44c1-203">Příspěvky jsou označeny jako změněné, protože severing relace způsobilo Cizíklíč byla označena jako hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="a44c1-203">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="a44c1-204">Pokud Cizíklíč není s možnou hodnotou Null, pak se skutečnou hodnotou nedojde ke změně i když je označen jako hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="a44c1-204">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="a44c1-205">SaveChanges, pokusí se nastavit post cizího klíče na hodnotu null, ale to nezdaří, protože není Cizíklíč s možnou hodnotou Null</span><span class="sxs-lookup"><span data-stu-id="a44c1-205">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="a44c1-206">DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s volitelný vztah</span><span class="sxs-lookup"><span data-stu-id="a44c1-206">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="a44c1-207">Příspěvky jsou označeny jako změněné, protože severing relace způsobilo Cizíklíč byla označena jako hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="a44c1-207">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="a44c1-208">Pokud Cizíklíč není s možnou hodnotou Null, pak se skutečnou hodnotou nedojde ke změně i když je označen jako hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="a44c1-208">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="a44c1-209">SaveChanges nastaví Cizíklíč obě položky závislé na nebo podřízených prvků (příspěvky) na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="a44c1-209">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="a44c1-210">Po uložení závislé objekty nebo podřízené objekty (příspěvky) teď mít hodnotu null. hodnoty cizího klíče a odebrala se jejich odkaz na odstraněného objektu zabezpečení nebo nadřízené (blog)</span><span class="sxs-lookup"><span data-stu-id="a44c1-210">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="a44c1-211">DeleteBehavior.Restrict s požadované nebo volitelné relace</span><span class="sxs-lookup"><span data-stu-id="a44c1-211">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="a44c1-212">Příspěvky jsou označeny jako změněné, protože severing relace způsobilo Cizíklíč byla označena jako hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="a44c1-212">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="a44c1-213">Pokud Cizíklíč není s možnou hodnotou Null, pak se skutečnou hodnotou nedojde ke změně i když je označen jako hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="a44c1-213">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="a44c1-214">Vzhledem k tomu *omezit* informuje EF automaticky nastavit cizího klíče na hodnotu null, zůstane jinou hodnotu než null a SaveChanges vyvolá bez uložení</span><span class="sxs-lookup"><span data-stu-id="a44c1-214">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="a44c1-215">Kaskádových na Nesledované entity</span><span class="sxs-lookup"><span data-stu-id="a44c1-215">Cascading to untracked entities</span></span>

<span data-ttu-id="a44c1-216">Při volání *SaveChanges*, cascade odstranit pravidla se použijí pro všechny entity, které jsou sledovány kontextu.</span><span class="sxs-lookup"><span data-stu-id="a44c1-216">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="a44c1-217">Toto je situaci ve všech příklady uvedené nahoře, který je důvod, proč SQL byl vygenerován odstranit objekt zabezpečení nebo nadřízené (blog) a všechny závislé objekty nebo podřízené objekty (příspěvky):</span><span class="sxs-lookup"><span data-stu-id="a44c1-217">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="a44c1-218">Pokud pouze objekt je načteno – například když se dotazuje pro blog bez `Include(b => b.Posts)` zahrnout také příspěvcích – pak SaveChanges vygeneruje jenom SQL se odstranit objekt zabezpečení nebo nadřízené:</span><span class="sxs-lookup"><span data-stu-id="a44c1-218">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="a44c1-219">Závislé objekty nebo podřízené objekty (příspěvky) budou odstraněny, pouze pokud databáze obsahuje odpovídající cascade chování nakonfigurovaném.</span><span class="sxs-lookup"><span data-stu-id="a44c1-219">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="a44c1-220">Pokud používáte EF k vytvoření databáze, bude toto chování cascade instalace pro vás.</span><span class="sxs-lookup"><span data-stu-id="a44c1-220">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
