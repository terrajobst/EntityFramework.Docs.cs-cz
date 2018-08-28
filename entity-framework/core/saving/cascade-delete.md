---
title: Odstranění – EF Core v kaskádě
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: afe00ddb1b487c6b1b2ea42708c9967a57cea04b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995239"
---
# <a name="cascade-delete"></a><span data-ttu-id="25e95-102">Kaskádové odstranění</span><span class="sxs-lookup"><span data-stu-id="25e95-102">Cascade Delete</span></span>

<span data-ttu-id="25e95-103">Kaskádové odstranění se běžně používá v, řečeno terminologií databáze k popisu znakem, který umožňuje odstranění řádku pro automatickou aktivaci odstranění související řádky.</span><span class="sxs-lookup"><span data-stu-id="25e95-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="25e95-104">Úzce související pojem také popsaná v EF Core odstranit chování je automatické odstranění podřízené entity, když jeho vztah k nadřazené bylo porušeno – to se běžně označuje jako "odstraňování osamocené položky".</span><span class="sxs-lookup"><span data-stu-id="25e95-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="25e95-105">EF Core implementuje několik různých odstranit chování a umožňuje konfigurovat chování odstranění jednotlivých vztahů.</span><span class="sxs-lookup"><span data-stu-id="25e95-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="25e95-106">EF Core také implementuje vytváření názvů, který automaticky nakonfiguruje odstranit užitečné výchozí chování pro každou relaci na základě [requiredness relace](../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="25e95-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="25e95-107">Odstranit chování</span><span class="sxs-lookup"><span data-stu-id="25e95-107">Delete behaviors</span></span>
<span data-ttu-id="25e95-108">Odstranit chování, které jsou definovány v *DeleteBehavior* enumerátor typu a mohou být předány *elementy OnDelete* fluent API pro ovládací prvek, zda odstranění objektu zabezpečení/nadřazená entita nebo severing z vztah k entity závislé/podřízený by měl mít vedlejší účinek v závislé/podřízené entity.</span><span class="sxs-lookup"><span data-stu-id="25e95-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="25e95-109">Existují tři akce, které EF můžete provést, když je odstraněn objekt zabezpečení/nadřazená entita nebo porušeno vztah podřízený:</span><span class="sxs-lookup"><span data-stu-id="25e95-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="25e95-110">Podřízené rolích dependent je možné odstranit.</span><span class="sxs-lookup"><span data-stu-id="25e95-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="25e95-111">Podřízené hodnoty cizího klíče můžete nastavit na hodnotu null</span><span class="sxs-lookup"><span data-stu-id="25e95-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="25e95-112">Podřízené zůstane beze změny</span><span class="sxs-lookup"><span data-stu-id="25e95-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="25e95-113">Odstranit chování nakonfigurovaném v modelu EF Core platí pouze při instančního objektu entity se odstraní pomocí EF Core a entity, které závislé jsou načtena do paměti (to znamená pro sledované závislosti).</span><span class="sxs-lookup"><span data-stu-id="25e95-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (that is, for tracked dependents).</span></span> <span data-ttu-id="25e95-114">Odpovídající chování cascade musí být, že má instalační program v databázi pro zajištění dat, která není sledována kontextu použité potřebné akce.</span><span class="sxs-lookup"><span data-stu-id="25e95-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="25e95-115">Pokud použijete k vytvoření databáze EF Core, bude toto chování cascade nastavení za vás.</span><span class="sxs-lookup"><span data-stu-id="25e95-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="25e95-116">Pro druhý výše uvedenou akci nastavení hodnoty cizího klíče na hodnotu null není platná, pokud není cizí klíč s možnou hodnotou Null.</span><span class="sxs-lookup"><span data-stu-id="25e95-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="25e95-117">(Je ekvivalentní povinný vztah cizího klíče nepřipouštějící.) V těchto případech EF Core sleduje, že vlastnost cizího klíče byl označen jako null dokud se nazývá SaveChanges, kdy je vyvolána výjimka, protože změny nelze nastavit jako trvalý do databáze.</span><span class="sxs-lookup"><span data-stu-id="25e95-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="25e95-118">Toto je podobný získávání porušení omezení z databáze.</span><span class="sxs-lookup"><span data-stu-id="25e95-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="25e95-119">Existují čtyři odstranit chování, jak je uvedeno v následujících tabulkách.</span><span class="sxs-lookup"><span data-stu-id="25e95-119">There are four delete behaviors, as listed in the tables below.</span></span>

### <a name="optional-relationships"></a><span data-ttu-id="25e95-120">Volitelné relace</span><span class="sxs-lookup"><span data-stu-id="25e95-120">Optional relationships</span></span>
<span data-ttu-id="25e95-121">Pro volitelné relace (s možnou hodnotou Null cizí klíč) ji _je_ možné uložit cizího klíče hodnotu null, což vede k následujících efektů:</span><span class="sxs-lookup"><span data-stu-id="25e95-121">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="25e95-122">Název chování</span><span class="sxs-lookup"><span data-stu-id="25e95-122">Behavior Name</span></span>               | <span data-ttu-id="25e95-123">Vliv na závislé a podřízené v paměti</span><span class="sxs-lookup"><span data-stu-id="25e95-123">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="25e95-124">Vliv na závislé/podřízené databáze</span><span class="sxs-lookup"><span data-stu-id="25e95-124">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="25e95-125">**Kaskádové**</span><span class="sxs-lookup"><span data-stu-id="25e95-125">**Cascade**</span></span>                 | <span data-ttu-id="25e95-126">Odstranění entit</span><span class="sxs-lookup"><span data-stu-id="25e95-126">Entities are deleted</span></span>                   | <span data-ttu-id="25e95-127">Odstranění entit</span><span class="sxs-lookup"><span data-stu-id="25e95-127">Entities are deleted</span></span>                   |
| <span data-ttu-id="25e95-128">**ClientSetNull** (výchozí)</span><span class="sxs-lookup"><span data-stu-id="25e95-128">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="25e95-129">Vlastnosti cizího klíče jsou nastaveny na hodnotu null</span><span class="sxs-lookup"><span data-stu-id="25e95-129">Foreign key properties are set to null</span></span> | <span data-ttu-id="25e95-130">Žádné</span><span class="sxs-lookup"><span data-stu-id="25e95-130">None</span></span>                                   |
| <span data-ttu-id="25e95-131">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="25e95-131">**SetNull**</span></span>                 | <span data-ttu-id="25e95-132">Vlastnosti cizího klíče jsou nastaveny na hodnotu null</span><span class="sxs-lookup"><span data-stu-id="25e95-132">Foreign key properties are set to null</span></span> | <span data-ttu-id="25e95-133">Vlastnosti cizího klíče jsou nastaveny na hodnotu null</span><span class="sxs-lookup"><span data-stu-id="25e95-133">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="25e95-134">**omezení**</span><span class="sxs-lookup"><span data-stu-id="25e95-134">**Restrict**</span></span>                | <span data-ttu-id="25e95-135">Žádné</span><span class="sxs-lookup"><span data-stu-id="25e95-135">None</span></span>                                   | <span data-ttu-id="25e95-136">Žádné</span><span class="sxs-lookup"><span data-stu-id="25e95-136">None</span></span>                                   |

### <a name="required-relationships"></a><span data-ttu-id="25e95-137">Požadovaná relace</span><span class="sxs-lookup"><span data-stu-id="25e95-137">Required relationships</span></span>
<span data-ttu-id="25e95-138">Požadovaná relace (neumožňující cizí klíč) je _není_ možné uložit cizího klíče hodnotu null, což vede k následujících efektů:</span><span class="sxs-lookup"><span data-stu-id="25e95-138">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="25e95-139">Název chování</span><span class="sxs-lookup"><span data-stu-id="25e95-139">Behavior Name</span></span>         | <span data-ttu-id="25e95-140">Vliv na závislé a podřízené v paměti</span><span class="sxs-lookup"><span data-stu-id="25e95-140">Effect on dependent/child in memory</span></span> | <span data-ttu-id="25e95-141">Vliv na závislé/podřízené databáze</span><span class="sxs-lookup"><span data-stu-id="25e95-141">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="25e95-142">**Kaskádové** (výchozí)</span><span class="sxs-lookup"><span data-stu-id="25e95-142">**Cascade** (Default)</span></span> | <span data-ttu-id="25e95-143">Odstranění entit</span><span class="sxs-lookup"><span data-stu-id="25e95-143">Entities are deleted</span></span>                | <span data-ttu-id="25e95-144">Odstranění entit</span><span class="sxs-lookup"><span data-stu-id="25e95-144">Entities are deleted</span></span>                  |
| <span data-ttu-id="25e95-145">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="25e95-145">**ClientSetNull**</span></span>     | <span data-ttu-id="25e95-146">Vyvolá SaveChanges</span><span class="sxs-lookup"><span data-stu-id="25e95-146">SaveChanges throws</span></span>                  | <span data-ttu-id="25e95-147">Žádné</span><span class="sxs-lookup"><span data-stu-id="25e95-147">None</span></span>                                  |
| <span data-ttu-id="25e95-148">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="25e95-148">**SetNull**</span></span>           | <span data-ttu-id="25e95-149">Vyvolá SaveChanges</span><span class="sxs-lookup"><span data-stu-id="25e95-149">SaveChanges throws</span></span>                  | <span data-ttu-id="25e95-150">Vyvolá SaveChanges</span><span class="sxs-lookup"><span data-stu-id="25e95-150">SaveChanges throws</span></span>                    |
| <span data-ttu-id="25e95-151">**omezení**</span><span class="sxs-lookup"><span data-stu-id="25e95-151">**Restrict**</span></span>          | <span data-ttu-id="25e95-152">Žádné</span><span class="sxs-lookup"><span data-stu-id="25e95-152">None</span></span>                                | <span data-ttu-id="25e95-153">Žádné</span><span class="sxs-lookup"><span data-stu-id="25e95-153">None</span></span>                                  |

<span data-ttu-id="25e95-154">V tabulkách výše *žádný* může vést k narušení omezení.</span><span class="sxs-lookup"><span data-stu-id="25e95-154">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="25e95-155">Například pokud se entita hlavní/podřízený se odstraní, ale chcete-li změnit cizí klíč závislé/podřízený nebyla provedena žádná akce, poté databázi pravděpodobně vyvolá na SaveChanges z důvodu narušení omezení pro cizí.</span><span class="sxs-lookup"><span data-stu-id="25e95-155">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="25e95-156">Na vysoké úrovni:</span><span class="sxs-lookup"><span data-stu-id="25e95-156">At a high level:</span></span>
* <span data-ttu-id="25e95-157">Pokud máte entity, které nemohou existovat bez nadřazených a EF postará za odstranění podřízené objekty automaticky a pak použijte *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="25e95-157">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="25e95-158">Entity, které nemohou existovat bez nadřazené obvykle provést pomocí požadované vztahů, pro kterou *Cascade* je výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="25e95-158">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="25e95-159">Pokud máte entity, které může nebo nemusí být nadřazenou položku a EF, která se stará o nulling si cizí klíč pro vás a pak použijte *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="25e95-159">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="25e95-160">Entity, které mohou existovat bez nadřazené obvykle provést použít volitelný vztahů, pro kterou *ClientSetNull* je výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="25e95-160">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="25e95-161">Pokud chcete databázi taky zkusit šíření hodnoty null k cizím klíčům podřízené i když není načten podřízené entity, pak použijte *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="25e95-161">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="25e95-162">Mějte však na paměti, že databáze musí podporovat to a konfigurace databáze tímto způsobem může způsobit další omezení, která v praxi často nepraktické tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="25e95-162">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="25e95-163">To je důvod, proč *SetNull* není výchozí.</span><span class="sxs-lookup"><span data-stu-id="25e95-163">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="25e95-164">Pokud nechcete, aby EF Core pro nikdy automaticky odstranit entitu nebo hodnota null, cizí klíč automaticky, a následné použití *omezit*.</span><span class="sxs-lookup"><span data-stu-id="25e95-164">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="25e95-165">Upozorňujeme, že to vyžaduje, aby váš kód synchronizaci podřízených entit a jejich hodnoty cizího klíče ručně jinak omezení výjimky budou vyvolány.</span><span class="sxs-lookup"><span data-stu-id="25e95-165">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="25e95-166">V EF Core, na rozdíl od EF6 kaskádové účinky neproběhne okamžitě, ale místo toho pouze když je volána metoda SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="25e95-166">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="25e95-167">**Změny v EF Core 2.0:** v předchozích verzích *omezit* by způsobilo volitelné vlastnosti cizího klíče v sledované závislé můžou být nastavena na hodnotu null a je výchozí chování pro volitelné relací při odstranění.</span><span class="sxs-lookup"><span data-stu-id="25e95-167">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="25e95-168">V EF Core 2.0 *ClientSetNull* byl zaveden k reprezentaci tohoto chování a začal na výchozí hodnoty pro nepovinný vztahy.</span><span class="sxs-lookup"><span data-stu-id="25e95-168">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="25e95-169">Chování *omezit* byla upravena nikdy mít žádné vedlejší účinky na závislých položek.</span><span class="sxs-lookup"><span data-stu-id="25e95-169">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="25e95-170">Příklady odstranění entity</span><span class="sxs-lookup"><span data-stu-id="25e95-170">Entity deletion examples</span></span>

<span data-ttu-id="25e95-171">Následující kód je součástí [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , který je možné stáhnout a spustit.</span><span class="sxs-lookup"><span data-stu-id="25e95-171">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="25e95-172">Vzorek ukazuje, co se stane pro každou chování při odstraňování u povinných a volitelných relací při odstranění nadřazená entita.</span><span class="sxs-lookup"><span data-stu-id="25e95-172">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="25e95-173">Projděme si každý variace pochopit, co se děje.</span><span class="sxs-lookup"><span data-stu-id="25e95-173">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="25e95-174">DeleteBehavior.Cascade požadované nebo volitelné vztah</span><span class="sxs-lookup"><span data-stu-id="25e95-174">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="25e95-175">Blog je označený jako odstraněný</span><span class="sxs-lookup"><span data-stu-id="25e95-175">Blog is marked as Deleted</span></span>
* <span data-ttu-id="25e95-176">Příspěvky zpočátku zůstat Unchanged, protože cascades neproběhne až do SaveChanges</span><span class="sxs-lookup"><span data-stu-id="25e95-176">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="25e95-177">SaveChanges odešle odstraní závislé položky a podřízené položky (příspěvků) a potom objekt zabezpečení/nadřazený (blog)</span><span class="sxs-lookup"><span data-stu-id="25e95-177">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="25e95-178">Po uložení se všechny entity odpojit, protože nyní z databáze byla odstraněna</span><span class="sxs-lookup"><span data-stu-id="25e95-178">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="25e95-179">DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s povinný vztah</span><span class="sxs-lookup"><span data-stu-id="25e95-179">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="25e95-180">Blog je označený jako odstraněný</span><span class="sxs-lookup"><span data-stu-id="25e95-180">Blog is marked as Deleted</span></span>
* <span data-ttu-id="25e95-181">Příspěvky zpočátku zůstat Unchanged, protože cascades neproběhne až do SaveChanges</span><span class="sxs-lookup"><span data-stu-id="25e95-181">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="25e95-182">SaveChanges, pokusí se nastavit příspěvek cizího klíče na hodnotu null, ale to se nezdaří, protože není FK s možnou hodnotou Null</span><span class="sxs-lookup"><span data-stu-id="25e95-182">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="25e95-183">DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s volitelný vztah</span><span class="sxs-lookup"><span data-stu-id="25e95-183">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="25e95-184">Blog je označený jako odstraněný</span><span class="sxs-lookup"><span data-stu-id="25e95-184">Blog is marked as Deleted</span></span>
* <span data-ttu-id="25e95-185">Příspěvky zpočátku zůstat Unchanged, protože cascades neproběhne až do SaveChanges</span><span class="sxs-lookup"><span data-stu-id="25e95-185">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="25e95-186">Pokusy o SaveChanges FK obě položky závislé na/podřízené prvky (příspěvků) nastaví na hodnotu null před odstraněním objektu zabezpečení/nadřazený (blog)</span><span class="sxs-lookup"><span data-stu-id="25e95-186">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="25e95-187">Po uložení objektu zabezpečení/nadřazený (blog) je odstraněn, ale závislé položky a podřízené objekty (příspěvků) jsou stále sledována.</span><span class="sxs-lookup"><span data-stu-id="25e95-187">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="25e95-188">Sledované závislé položky a podřízené objekty (příspěvků) teď obsahují hodnoty null cizího klíče a jejich odkaz na odstraněný objekt zabezpečení/nadřazený (blog) byla odebrána.</span><span class="sxs-lookup"><span data-stu-id="25e95-188">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="25e95-189">DeleteBehavior.Restrict požadované nebo volitelné vztah</span><span class="sxs-lookup"><span data-stu-id="25e95-189">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="25e95-190">Blog je označený jako odstraněný</span><span class="sxs-lookup"><span data-stu-id="25e95-190">Blog is marked as Deleted</span></span>
* <span data-ttu-id="25e95-191">Příspěvky zpočátku zůstat Unchanged, protože cascades neproběhne až do SaveChanges</span><span class="sxs-lookup"><span data-stu-id="25e95-191">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="25e95-192">Protože *omezit* říká EF automaticky nastavit cizího klíče na hodnotu null, zůstane jinou hodnotu než null a SaveChanges vyvolá bez uložení</span><span class="sxs-lookup"><span data-stu-id="25e95-192">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="25e95-193">Odstranit příklady osamocené položky</span><span class="sxs-lookup"><span data-stu-id="25e95-193">Delete orphans examples</span></span>

<span data-ttu-id="25e95-194">Následující kód je součástí [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , který je možné stáhnout a spustit.</span><span class="sxs-lookup"><span data-stu-id="25e95-194">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="25e95-195">Vzorek ukazuje, co se stane pro každou chování při odstraňování u povinných a volitelných relací při porušeno vztah nadřazeného/instanční objekt a jeho podřízené objekty/dependents.</span><span class="sxs-lookup"><span data-stu-id="25e95-195">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="25e95-196">V tomto příkladu je vztah porušeno odebráním závislé položky a podřízené objekty (příspěvků) z navigační vlastnost kolekce na objekt zabezpečení/nadřazený (blog).</span><span class="sxs-lookup"><span data-stu-id="25e95-196">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="25e95-197">Chování je však stejný Pokud odkaz z závislé/podřízený na objekt zabezpečení/nadřazený prvek je místo toho vynulovanými navýšení kapacity.</span><span class="sxs-lookup"><span data-stu-id="25e95-197">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="25e95-198">Projděme si každý variace pochopit, co se děje.</span><span class="sxs-lookup"><span data-stu-id="25e95-198">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="25e95-199">DeleteBehavior.Cascade požadované nebo volitelné vztah</span><span class="sxs-lookup"><span data-stu-id="25e95-199">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="25e95-200">Příspěvky jsou označeny jako změněno, protože severing vztah způsobily FK být označený jako null</span><span class="sxs-lookup"><span data-stu-id="25e95-200">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="25e95-201">Pokud FK není s možnou hodnotou Null, pak skutečnou hodnotu nedojde ke změně i v případě, že je označena jako hodnota null</span><span class="sxs-lookup"><span data-stu-id="25e95-201">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="25e95-202">SaveChanges odešle odstraní pro závislé položky a podřízené položky (příspěvků)</span><span class="sxs-lookup"><span data-stu-id="25e95-202">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="25e95-203">Po uložení, jsou závislé položky a podřízené objekty (příspěvků) odpojit, protože nyní z databáze byla odstraněna</span><span class="sxs-lookup"><span data-stu-id="25e95-203">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="25e95-204">DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s povinný vztah</span><span class="sxs-lookup"><span data-stu-id="25e95-204">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="25e95-205">Příspěvky jsou označeny jako změněno, protože severing vztah způsobily FK být označený jako null</span><span class="sxs-lookup"><span data-stu-id="25e95-205">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="25e95-206">Pokud FK není s možnou hodnotou Null, pak skutečnou hodnotu nedojde ke změně i v případě, že je označena jako hodnota null</span><span class="sxs-lookup"><span data-stu-id="25e95-206">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="25e95-207">SaveChanges, pokusí se nastavit příspěvek cizího klíče na hodnotu null, ale to se nezdaří, protože není FK s možnou hodnotou Null</span><span class="sxs-lookup"><span data-stu-id="25e95-207">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="25e95-208">DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s volitelný vztah</span><span class="sxs-lookup"><span data-stu-id="25e95-208">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="25e95-209">Příspěvky jsou označeny jako změněno, protože severing vztah způsobily FK být označený jako null</span><span class="sxs-lookup"><span data-stu-id="25e95-209">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="25e95-210">Pokud FK není s možnou hodnotou Null, pak skutečnou hodnotu nedojde ke změně i v případě, že je označena jako hodnota null</span><span class="sxs-lookup"><span data-stu-id="25e95-210">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="25e95-211">SaveChanges FK obě položky závislé na/podřízené prvky (příspěvků) nastaví na hodnotu null</span><span class="sxs-lookup"><span data-stu-id="25e95-211">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="25e95-212">Po uložení, závislé položky a podřízené objekty (příspěvků) teď obsahují hodnoty null cizího klíče a jejich odkaz na odstraněný objekt zabezpečení/nadřazený (blog) byla odebrána.</span><span class="sxs-lookup"><span data-stu-id="25e95-212">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="25e95-213">DeleteBehavior.Restrict požadované nebo volitelné vztah</span><span class="sxs-lookup"><span data-stu-id="25e95-213">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="25e95-214">Příspěvky jsou označeny jako změněno, protože severing vztah způsobily FK být označený jako null</span><span class="sxs-lookup"><span data-stu-id="25e95-214">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="25e95-215">Pokud FK není s možnou hodnotou Null, pak skutečnou hodnotu nedojde ke změně i v případě, že je označena jako hodnota null</span><span class="sxs-lookup"><span data-stu-id="25e95-215">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="25e95-216">Protože *omezit* říká EF automaticky nastavit cizího klíče na hodnotu null, zůstane jinou hodnotu než null a SaveChanges vyvolá bez uložení</span><span class="sxs-lookup"><span data-stu-id="25e95-216">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="25e95-217">Kaskádová šablona Nesledované entity</span><span class="sxs-lookup"><span data-stu-id="25e95-217">Cascading to untracked entities</span></span>

<span data-ttu-id="25e95-218">Při volání *SaveChanges*, kaskádové odstranění pravidla se použijí pro všechny entity, které jsou sledovány kontextu.</span><span class="sxs-lookup"><span data-stu-id="25e95-218">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="25e95-219">Toto je situace, ve všech příkladů uvedených výše, což je důvod, proč SQL se vygeneroval se odstranit objekt zabezpečení/nadřazený (blog) a všechny závislé položky a podřízené objekty (příspěvků):</span><span class="sxs-lookup"><span data-stu-id="25e95-219">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="25e95-220">Pokud pouze pro objekt zabezpečení je načteno – například když se provede dotaz pro blog bez `Include(b => b.Posts)` Pokud chcete také zahrnout příspěvky – potom SaveChanges vygeneruje jenom SQL se odstranit objekt zabezpečení/nadřazený prvek:</span><span class="sxs-lookup"><span data-stu-id="25e95-220">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="25e95-221">Závislé položky a podřízené objekty (příspěvků) budou odstraněny, pouze pokud databáze nemá odpovídající chování cascade nakonfigurovaném.</span><span class="sxs-lookup"><span data-stu-id="25e95-221">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="25e95-222">Pokud použijete k vytvoření databáze EF, bude toto chování cascade nastavení za vás.</span><span class="sxs-lookup"><span data-stu-id="25e95-222">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
