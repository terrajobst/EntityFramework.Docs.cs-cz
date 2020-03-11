---
title: Kaskádové odstranění – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: 6e92b869d691d0224abf1997d9eb7ea035489c5d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417612"
---
# <a name="cascade-delete"></a><span data-ttu-id="2f14f-102">Kaskádové odstranění</span><span class="sxs-lookup"><span data-stu-id="2f14f-102">Cascade Delete</span></span>

<span data-ttu-id="2f14f-103">Kaskádové odstraňování se často používá v terminologii databáze k popisu charakteristiky, která umožňuje odstranění řádku automaticky aktivovat odstranění souvisejících řádků.</span><span class="sxs-lookup"><span data-stu-id="2f14f-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="2f14f-104">Úzce související pojem, který je pokrytý EF Core odstranění chování je automatické odstranění podřízené entity, pokud je relace vůči nadřazenému objektu nadřazená – to se obvykle označuje jako "odstranění osamocení".</span><span class="sxs-lookup"><span data-stu-id="2f14f-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="2f14f-105">EF Core implementuje několik různých chování při odstraňování a umožňuje konfiguraci chování při odstraňování jednotlivých vztahů.</span><span class="sxs-lookup"><span data-stu-id="2f14f-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="2f14f-106">EF Core také implementuje konvence, které automaticky nakonfigurují užitečné výchozí chování při odstraňování každého vztahu na základě [požadované hodnoty vztahu](../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="2f14f-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="2f14f-107">Chování při odstranění</span><span class="sxs-lookup"><span data-stu-id="2f14f-107">Delete behaviors</span></span>

<span data-ttu-id="2f14f-108">Chování při odstranění jsou definovaná v typu enumerátoru *DeleteBehavior* a dají se předat rozhraní API pro *odstranění* Fluent, abyste mohli řídit, jestli odstranění objektu zabezpečení nebo nadřazené entity nebo závažnosti vztahu k závislým nebo podřízeným entitám má mít vedlejší vliv na závislé nebo podřízené entity.</span><span class="sxs-lookup"><span data-stu-id="2f14f-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="2f14f-109">Existují tři akce, které EF může provést při odstranění objektu zabezpečení nebo nadřazené entity nebo při jeho vztahu k podřízenému objektu:</span><span class="sxs-lookup"><span data-stu-id="2f14f-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>

* <span data-ttu-id="2f14f-110">Podřízená/závislá je možné odstranit.</span><span class="sxs-lookup"><span data-stu-id="2f14f-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="2f14f-111">Hodnoty cizího klíče dítěte mohou být nastaveny na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="2f14f-112">Podřízená položka zůstane beze změny.</span><span class="sxs-lookup"><span data-stu-id="2f14f-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="2f14f-113">Chování při odstranění, které je nakonfigurované v modelu EF Core, se použije jenom v případě, že je primární entita Odstraněná pomocí EF Core a závislé entity se načtou do paměti (tj. pro sledované závislé položky).</span><span class="sxs-lookup"><span data-stu-id="2f14f-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (that is, for tracked dependents).</span></span> <span data-ttu-id="2f14f-114">V databázi musí být nastavené odpovídající chování funkce Cascade, aby se zajistilo, že data, která nejsou sledována kontextem, jsou použita potřebná akce.</span><span class="sxs-lookup"><span data-stu-id="2f14f-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="2f14f-115">Pokud k vytvoření databáze použijete EF Core, bude chování kaskádového nastavení pro vás.</span><span class="sxs-lookup"><span data-stu-id="2f14f-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="2f14f-116">U druhé akce výše nastavení hodnota cizího klíče na hodnotu null není platné, pokud cizí klíč nemůže mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="2f14f-117">(Cizí klíč, který nemůže mít hodnotu null, je ekvivalentní k požadované relaci.) V těchto případech EF Core sleduje, že vlastnost cizího klíče byla označena jako null, dokud není volána metoda SaveChanges, přičemž v této době je vyvolána výjimka, protože změnu nelze trvale uložit do databáze.</span><span class="sxs-lookup"><span data-stu-id="2f14f-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="2f14f-118">To je podobné jako získání porušení omezení z databáze.</span><span class="sxs-lookup"><span data-stu-id="2f14f-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="2f14f-119">Existují čtyři chování při odstranění, jak je uvedeno v následujících tabulkách.</span><span class="sxs-lookup"><span data-stu-id="2f14f-119">There are four delete behaviors, as listed in the tables below.</span></span>

### <a name="optional-relationships"></a><span data-ttu-id="2f14f-120">Volitelné relace</span><span class="sxs-lookup"><span data-stu-id="2f14f-120">Optional relationships</span></span>

<span data-ttu-id="2f14f-121">U volitelných vztahů (cizí klíč s možnou hodnotou null) _je_ možné uložit hodnotu cizího klíče s hodnotou null, která má za následek následující důsledky:</span><span class="sxs-lookup"><span data-stu-id="2f14f-121">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="2f14f-122">Název chování</span><span class="sxs-lookup"><span data-stu-id="2f14f-122">Behavior Name</span></span>               | <span data-ttu-id="2f14f-123">Vliv na závislé nebo podřízené objekty v paměti</span><span class="sxs-lookup"><span data-stu-id="2f14f-123">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="2f14f-124">Vliv na závislé nebo podřízené v databázi</span><span class="sxs-lookup"><span data-stu-id="2f14f-124">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="2f14f-125">**Nášejí**</span><span class="sxs-lookup"><span data-stu-id="2f14f-125">**Cascade**</span></span>                 | <span data-ttu-id="2f14f-126">Entity jsou odstraněny</span><span class="sxs-lookup"><span data-stu-id="2f14f-126">Entities are deleted</span></span>                   | <span data-ttu-id="2f14f-127">Entity jsou odstraněny</span><span class="sxs-lookup"><span data-stu-id="2f14f-127">Entities are deleted</span></span>                   |
| <span data-ttu-id="2f14f-128">**ClientSetNull** (výchozí)</span><span class="sxs-lookup"><span data-stu-id="2f14f-128">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="2f14f-129">Vlastnosti cizího klíče jsou nastaveny na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-129">Foreign key properties are set to null</span></span> | <span data-ttu-id="2f14f-130">Žádná</span><span class="sxs-lookup"><span data-stu-id="2f14f-130">None</span></span>                                   |
| <span data-ttu-id="2f14f-131">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="2f14f-131">**SetNull**</span></span>                 | <span data-ttu-id="2f14f-132">Vlastnosti cizího klíče jsou nastaveny na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-132">Foreign key properties are set to null</span></span> | <span data-ttu-id="2f14f-133">Vlastnosti cizího klíče jsou nastaveny na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-133">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="2f14f-134">**Omezit**</span><span class="sxs-lookup"><span data-stu-id="2f14f-134">**Restrict**</span></span>                | <span data-ttu-id="2f14f-135">Žádná</span><span class="sxs-lookup"><span data-stu-id="2f14f-135">None</span></span>                                   | <span data-ttu-id="2f14f-136">Žádná</span><span class="sxs-lookup"><span data-stu-id="2f14f-136">None</span></span>                                   |

### <a name="required-relationships"></a><span data-ttu-id="2f14f-137">Požadované relace</span><span class="sxs-lookup"><span data-stu-id="2f14f-137">Required relationships</span></span>

<span data-ttu-id="2f14f-138">Pro požadované relace (cizí klíč, který nesmí mít hodnotu null), _není_ možné uložit hodnotu cizího klíče s hodnotou null, která má za následek následující důsledky:</span><span class="sxs-lookup"><span data-stu-id="2f14f-138">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="2f14f-139">Název chování</span><span class="sxs-lookup"><span data-stu-id="2f14f-139">Behavior Name</span></span>         | <span data-ttu-id="2f14f-140">Vliv na závislé nebo podřízené objekty v paměti</span><span class="sxs-lookup"><span data-stu-id="2f14f-140">Effect on dependent/child in memory</span></span> | <span data-ttu-id="2f14f-141">Vliv na závislé nebo podřízené v databázi</span><span class="sxs-lookup"><span data-stu-id="2f14f-141">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="2f14f-142">**Kaskádová** (výchozí)</span><span class="sxs-lookup"><span data-stu-id="2f14f-142">**Cascade** (Default)</span></span> | <span data-ttu-id="2f14f-143">Entity jsou odstraněny</span><span class="sxs-lookup"><span data-stu-id="2f14f-143">Entities are deleted</span></span>                | <span data-ttu-id="2f14f-144">Entity jsou odstraněny</span><span class="sxs-lookup"><span data-stu-id="2f14f-144">Entities are deleted</span></span>                  |
| <span data-ttu-id="2f14f-145">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="2f14f-145">**ClientSetNull**</span></span>     | <span data-ttu-id="2f14f-146">SaveChanges vyvolá</span><span class="sxs-lookup"><span data-stu-id="2f14f-146">SaveChanges throws</span></span>                  | <span data-ttu-id="2f14f-147">Žádná</span><span class="sxs-lookup"><span data-stu-id="2f14f-147">None</span></span>                                  |
| <span data-ttu-id="2f14f-148">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="2f14f-148">**SetNull**</span></span>           | <span data-ttu-id="2f14f-149">SaveChanges vyvolá</span><span class="sxs-lookup"><span data-stu-id="2f14f-149">SaveChanges throws</span></span>                  | <span data-ttu-id="2f14f-150">SaveChanges vyvolá</span><span class="sxs-lookup"><span data-stu-id="2f14f-150">SaveChanges throws</span></span>                    |
| <span data-ttu-id="2f14f-151">**Omezit**</span><span class="sxs-lookup"><span data-stu-id="2f14f-151">**Restrict**</span></span>          | <span data-ttu-id="2f14f-152">Žádná</span><span class="sxs-lookup"><span data-stu-id="2f14f-152">None</span></span>                                | <span data-ttu-id="2f14f-153">Žádná</span><span class="sxs-lookup"><span data-stu-id="2f14f-153">None</span></span>                                  |

<span data-ttu-id="2f14f-154">V tabulkách uvedených výše může *žádný* z nich dojít k porušení omezení.</span><span class="sxs-lookup"><span data-stu-id="2f14f-154">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="2f14f-155">Například pokud je odstraněna objekt hlavní/podřízená entita, ale není provedena žádná akce pro změnu cizího klíče závislého nebo podřízeného objektu, databáze pravděpodobně vyvolá operaci SaveChanges z důvodu narušení cizího omezení.</span><span class="sxs-lookup"><span data-stu-id="2f14f-155">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="2f14f-156">Na nejvyšší úrovni:</span><span class="sxs-lookup"><span data-stu-id="2f14f-156">At a high level:</span></span>

* <span data-ttu-id="2f14f-157">Pokud máte entity, které nemůžou existovat bez nadřazeného objektu, a chcete, aby se EF postaral o automatické odstranění podřízených objektů, použijte *kaskády*.</span><span class="sxs-lookup"><span data-stu-id="2f14f-157">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="2f14f-158">Entity, které nemohou existovat bez nadřazeného objektu obvykle využívají požadované relace, pro které je výchozí hodnota *Cascade* .</span><span class="sxs-lookup"><span data-stu-id="2f14f-158">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="2f14f-159">Pokud máte entity, které mohou nebo nemusí mít nadřazenou položku a chcete, aby EF postaral o vynulování cizího klíče za vás, pak použijte *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="2f14f-159">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="2f14f-160">Entity, které mohou existovat bez nadřazeného objektu obvykle využívají volitelné vztahy, pro které je výchozí hodnota *ClientSetNull* .</span><span class="sxs-lookup"><span data-stu-id="2f14f-160">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="2f14f-161">Pokud chcete, aby se databáze také pokusila rozšířit hodnoty null na podřízené cizí klíče i v případě, že není načtena podřízená entita, použijte *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="2f14f-161">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="2f14f-162">Uvědomte si však, že databáze musí podporovat tuto databázi, a pokud ji nakonfigurujete takto, může to mít za následek i jiná omezení, což v praxi často tuto možnost způsobuje nepraktické.</span><span class="sxs-lookup"><span data-stu-id="2f14f-162">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="2f14f-163">To je důvod, proč *SetNull* není výchozí.</span><span class="sxs-lookup"><span data-stu-id="2f14f-163">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="2f14f-164">Pokud nechcete, aby EF Core nikdy odstranit entitu automaticky, nebo vynulování cizího klíče automaticky vyprší, použijte možnost *omezit*.</span><span class="sxs-lookup"><span data-stu-id="2f14f-164">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="2f14f-165">Všimněte si, že to vyžaduje, aby váš kód zachoval podřízené entity a jejich hodnoty cizího klíče v synchronizaci ručně, jinak budou vyvolány výjimky omezení.</span><span class="sxs-lookup"><span data-stu-id="2f14f-165">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="2f14f-166">V EF Core na rozdíl od EF6 se kaskádové efekty okamžitě neprojeví, ale pouze v případě, že je volána metoda SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="2f14f-166">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="2f14f-167">**Změny v EF Core 2,0:** V předchozích verzích by *omezení* způsobilo, že volitelné vlastnosti cizího klíče ve sledovaných závislých entitách budou nastaveny na hodnotu null a bylo výchozím chováním při odstraňování volitelných vztahů.</span><span class="sxs-lookup"><span data-stu-id="2f14f-167">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="2f14f-168">V EF Core 2,0 byla zavedena *ClientSetNull* , která představuje toto chování a stala se výchozími pro volitelné relace.</span><span class="sxs-lookup"><span data-stu-id="2f14f-168">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="2f14f-169">Chování *omezení* bylo upraveno tak, aby na závislých entitách nikdy neměly žádné vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="2f14f-169">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="2f14f-170">Příklady odstranění entit</span><span class="sxs-lookup"><span data-stu-id="2f14f-170">Entity deletion examples</span></span>

<span data-ttu-id="2f14f-171">Níže uvedený kód je součástí [ukázky](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) , kterou je možné stáhnout a spustit.</span><span class="sxs-lookup"><span data-stu-id="2f14f-171">The code below is part of a [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="2f14f-172">Ukázka ukazuje, co se stane pro každé chování při odstraňování volitelných i požadovaných relací při odstranění nadřazené entity.</span><span class="sxs-lookup"><span data-stu-id="2f14f-172">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="2f14f-173">Pojďme si projít každou variantou, abychom porozuměli tomu, co se děje.</span><span class="sxs-lookup"><span data-stu-id="2f14f-173">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="2f14f-174">DeleteBehavior. Cascade s povinným nebo volitelným vztahem</span><span class="sxs-lookup"><span data-stu-id="2f14f-174">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="2f14f-175">Blog je označený jako odstraněný.</span><span class="sxs-lookup"><span data-stu-id="2f14f-175">Blog is marked as Deleted</span></span>
* <span data-ttu-id="2f14f-176">Příspěvky zpočátku zůstávají beze změny, protože kaskády neprobíhá, dokud SaveChanges neproběhne.</span><span class="sxs-lookup"><span data-stu-id="2f14f-176">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="2f14f-177">SaveChanges odesílá odstranění pro závislé položky i podřízené objekty (příspěvky) a pak hlavní/nadřazený (blog).</span><span class="sxs-lookup"><span data-stu-id="2f14f-177">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="2f14f-178">Po uložení se všechny entity odpojí od chvíle, kdy byly z databáze odstraněny.</span><span class="sxs-lookup"><span data-stu-id="2f14f-178">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="2f14f-179">DeleteBehavior. ClientSetNull nebo DeleteBehavior. SetNull s požadovanou relací</span><span class="sxs-lookup"><span data-stu-id="2f14f-179">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="2f14f-180">Blog je označený jako odstraněný.</span><span class="sxs-lookup"><span data-stu-id="2f14f-180">Blog is marked as Deleted</span></span>
* <span data-ttu-id="2f14f-181">Příspěvky zpočátku zůstávají beze změny, protože kaskády neprobíhá, dokud SaveChanges neproběhne.</span><span class="sxs-lookup"><span data-stu-id="2f14f-181">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="2f14f-182">Metoda SaveChanges se pokusí nastavit hodnotu post FK na null, ale to se nepovede, protože hodnota FK nemůže mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-182">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="2f14f-183">DeleteBehavior. ClientSetNull nebo DeleteBehavior. SetNull s volitelným vztahem</span><span class="sxs-lookup"><span data-stu-id="2f14f-183">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="2f14f-184">Blog je označený jako odstraněný.</span><span class="sxs-lookup"><span data-stu-id="2f14f-184">Blog is marked as Deleted</span></span>
* <span data-ttu-id="2f14f-185">Příspěvky zpočátku zůstávají beze změny, protože kaskády neprobíhá, dokud SaveChanges neproběhne.</span><span class="sxs-lookup"><span data-stu-id="2f14f-185">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="2f14f-186">Při pokusu o použití metody SaveChanges se před odstraněním objektu zabezpečení nebo nadřazené položky (blog) nastaví FK (post) na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-186">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="2f14f-187">Po uložení se objekt zabezpečení nebo nadřazený prvek (blog) odstraní, ale závislé objekty a podřízené položky (příspěvky) jsou pořád sledovány.</span><span class="sxs-lookup"><span data-stu-id="2f14f-187">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="2f14f-188">Sledované závislé položky/podřízené položky (příspěvky) nyní mají hodnoty neprázdných CK a jejich odkaz na odstraněný objekt zabezpečení nebo nadřazený objekt (blog) byl odebrán.</span><span class="sxs-lookup"><span data-stu-id="2f14f-188">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="2f14f-189">DeleteBehavior. restrict s povinným nebo volitelným vztahem</span><span class="sxs-lookup"><span data-stu-id="2f14f-189">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="2f14f-190">Blog je označený jako odstraněný.</span><span class="sxs-lookup"><span data-stu-id="2f14f-190">Blog is marked as Deleted</span></span>
* <span data-ttu-id="2f14f-191">Příspěvky zpočátku zůstávají beze změny, protože kaskády neprobíhá, dokud SaveChanges neproběhne.</span><span class="sxs-lookup"><span data-stu-id="2f14f-191">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="2f14f-192">Vzhledem k tomu, že příkaz *restrict* instruuje EF k automatickému nastavení hodnoty FK na hodnotu null, zůstává nenulová a možnost SaveChanges vyvolá bez uložení.</span><span class="sxs-lookup"><span data-stu-id="2f14f-192">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="2f14f-193">Odstranit osamocené příklady</span><span class="sxs-lookup"><span data-stu-id="2f14f-193">Delete orphans examples</span></span>

<span data-ttu-id="2f14f-194">Níže uvedený kód je součástí [ukázky](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) , kterou je možné stáhnout a spustit.</span><span class="sxs-lookup"><span data-stu-id="2f14f-194">The code below is part of a [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="2f14f-195">Ukázka ukazuje, co se stane pro každé chování při odstraňování volitelných i požadovaných vztahů, pokud je v relaci mezi nadřazeným objektem, hlavním prvkem a jeho podřízenými a závislými prvky vážně.</span><span class="sxs-lookup"><span data-stu-id="2f14f-195">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="2f14f-196">V tomto příkladu je relace závažná odebráním závislých a podřízených prvků (příspěvků) z navigační vlastnosti kolekce na objektu hlavní nebo nadřazené (blog).</span><span class="sxs-lookup"><span data-stu-id="2f14f-196">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="2f14f-197">Chování je však stejné, pokud je odkaz z závislého nebo podřízeného objektu na objekt zabezpečení nebo nadřazený je namísto hodnoty null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-197">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="2f14f-198">Pojďme si projít každou variantou, abychom porozuměli tomu, co se děje.</span><span class="sxs-lookup"><span data-stu-id="2f14f-198">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="2f14f-199">DeleteBehavior. Cascade s povinným nebo volitelným vztahem</span><span class="sxs-lookup"><span data-stu-id="2f14f-199">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="2f14f-200">Příspěvky jsou označeny jako upravené, protože závažnost vztahu způsobila, že hodnota FK je označena jako null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-200">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="2f14f-201">Pokud hodnota FK není null, skutečná hodnota se nemění, i když je označena jako null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-201">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="2f14f-202">SaveChanges odesílá odstranění pro závislé objekty/podřízené položky (příspěvky).</span><span class="sxs-lookup"><span data-stu-id="2f14f-202">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="2f14f-203">Po uložení se závislé položky nebo podřízené položky (příspěvky) odpojí od okamžiku odstranění z databáze.</span><span class="sxs-lookup"><span data-stu-id="2f14f-203">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="2f14f-204">DeleteBehavior. ClientSetNull nebo DeleteBehavior. SetNull s požadovanou relací</span><span class="sxs-lookup"><span data-stu-id="2f14f-204">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="2f14f-205">Příspěvky jsou označeny jako upravené, protože závažnost vztahu způsobila, že hodnota FK je označena jako null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-205">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="2f14f-206">Pokud hodnota FK není null, skutečná hodnota se nemění, i když je označena jako null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-206">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="2f14f-207">Metoda SaveChanges se pokusí nastavit hodnotu post FK na null, ale to se nepovede, protože hodnota FK nemůže mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-207">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="2f14f-208">DeleteBehavior. ClientSetNull nebo DeleteBehavior. SetNull s volitelným vztahem</span><span class="sxs-lookup"><span data-stu-id="2f14f-208">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="2f14f-209">Příspěvky jsou označeny jako upravené, protože závažnost vztahu způsobila, že hodnota FK je označena jako null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-209">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="2f14f-210">Pokud hodnota FK není null, skutečná hodnota se nemění, i když je označena jako null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-210">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="2f14f-211">SaveChanges nastaví FK pro závislé objekty nebo podřízené objekty (post) na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-211">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="2f14f-212">Po uložení budou mít následníci/podřízené (příspěvky) nyní hodnoty neprázdných hodnot FK a jejich odkaz na odstraněný objekt zabezpečení nebo nadřazený objekt (blog) byl odebrán.</span><span class="sxs-lookup"><span data-stu-id="2f14f-212">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="2f14f-213">DeleteBehavior. restrict s povinným nebo volitelným vztahem</span><span class="sxs-lookup"><span data-stu-id="2f14f-213">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="2f14f-214">Příspěvky jsou označeny jako upravené, protože závažnost vztahu způsobila, že hodnota FK je označena jako null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-214">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="2f14f-215">Pokud hodnota FK není null, skutečná hodnota se nemění, i když je označena jako null.</span><span class="sxs-lookup"><span data-stu-id="2f14f-215">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="2f14f-216">Vzhledem k tomu, že příkaz *restrict* instruuje EF k automatickému nastavení hodnoty FK na hodnotu null, zůstává nenulová a možnost SaveChanges vyvolá bez uložení.</span><span class="sxs-lookup"><span data-stu-id="2f14f-216">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="2f14f-217">Kaskádové k nesledovaným entitám</span><span class="sxs-lookup"><span data-stu-id="2f14f-217">Cascading to untracked entities</span></span>

<span data-ttu-id="2f14f-218">Při volání *metody SaveChanges*budou pravidla pro odstranění kaskádových pravidel použita u všech entit, které jsou sledovány v kontextu.</span><span class="sxs-lookup"><span data-stu-id="2f14f-218">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="2f14f-219">Jedná se o situaci ve všech výše uvedených příkladech, což je důvod, proč byl vygenerován SQL, aby odstranil hlavní nebo nadřazený objekt (blog) a všechny závislé prvky/podřízené položky (příspěvky):</span><span class="sxs-lookup"><span data-stu-id="2f14f-219">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="2f14f-220">Pokud je načtený jenom objekt zabezpečení (například když je na blogu vytvořený dotaz) bez `Include(b => b.Posts)`, aby taky zahrnoval příspěvky, pak SaveChanges vygeneruje SQL jenom pro odstranění objektu zabezpečení nebo nadřazené položky:</span><span class="sxs-lookup"><span data-stu-id="2f14f-220">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="2f14f-221">Závislé objekty a podřízené položky (příspěvky) se odstraní pouze v případě, že má databáze nakonfigurované odpovídající chování kaskádového chování.</span><span class="sxs-lookup"><span data-stu-id="2f14f-221">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="2f14f-222">Pokud k vytvoření databáze použijete EF, toto chování kaskádové instalace bude nastaveno za vás.</span><span class="sxs-lookup"><span data-stu-id="2f14f-222">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
