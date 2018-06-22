---
title: Odstranění – základní EF v kaskádě
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: 0fc8929c56d4c657b7fb1e3c8e4b1a71659220c9
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812674"
---
# <a name="cascade-delete"></a>Kaskádové odstranění

Kaskádové odstranění se běžně používá v terminologii databáze k popisu vlastnosti, který umožňuje odstranění řádku automaticky spouštět odstranění související řádky. Úzce související koncept také předmětem EF základní odstranit chování je automatické odstranění entita na podřízené při jeho vztah s nadřazenou položkou bylo porušeno – to se běžně označuje jako "odstranění osamocené položky".

Základní EF implementuje několik různých odstranit chování a umožňuje konfigurovat chování odstranění jednotlivých relací. Základní EF také implementuje konvence, který automaticky nakonfiguruje užitečné výchozí chování delete pro každý vztah na základě [requiredness relace](../modeling/relationships.md#required-and-optional-relationships).

## <a name="delete-behaviors"></a>Odstranit chování
Odstranit chování jsou definovány v *DeleteBehavior* enumerátor typu a se dá předat do *OnDelete* rozhraní fluent API na ovládací prvek zda odstranění hlavní/nadřazená entita nebo severing z vztah k závislé nebo podřízených entit by měl mít vedlejším účinkem závislé nebo podřízených entit.

Existují tři akce, kterou EF můžete provést, pokud je odstraněný hlavní/nadřazená entita, nebo je porušeno relace k podřízené:
* Podřízené/závislé mohl být odstraněn.
* Cizí klíče hodnoty dítěte lze nastavit na hodnotu null
* Podřízená zůstává beze změny

> [!NOTE]  
> Odstranění chování nakonfigurovaný v základní EF modelu je použít jenom v případě hlavní entity se odstraní pomocí EF jádra a závislé entity jsou načtena do paměti (tj. pro položky závislé na sledovaných). Odpovídající chování cascade musí být, že má instalační program v databázi, aby data, která není sledována kontextu potřebné akce použitá. Pokud používáte základní EF k vytvoření databáze, bude toto chování cascade instalace pro vás.

Druhou akci výše uvedených nastavení hodnoty cizího klíče na hodnotu null není platný, pokud cizí klíč není null. (Použití hodnot Null cizí klíč, který je ekvivalentní požadovaná relace.) V těchto případech EF základní sleduje, zda vlastností cizího klíče byl označen jako hodnotu null. dokud se nazývá SaveChanges, po kterých je vyvolána výjimka, protože změna nelze nastavit jako trvalý, do databáze. Toto je podobná získávání porušení omezení z databáze.

Existují čtyři odstranit chování, jak je uvedené v následujících tabulkách. Pro volitelné relace (s možnou hodnotou Null cizí klíč) je _je_ možné ukládat cizího klíče hodnotu null, což vede k tyto důsledky:

| Název chování               | Vliv na závislé a podřízené v paměti    | Vliv na závislé a podřízené databáze  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| **CASCADE**                 | Entity se odstraní.                   | Entity se odstraní.                   |
| **ClientSetNull** (výchozí) | Vlastnosti cizího klíče jsou nastaveny na hodnotu null | Žádné                                   |
| **SetNull**                 | Vlastnosti cizího klíče jsou nastaveny na hodnotu null | Vlastnosti cizího klíče jsou nastaveny na hodnotu null |
| **Omezení**                | Žádné                                   | Žádné                                   |

Pro požadované relace (použití hodnot Null cizí klíč) je _není_ možné ukládat cizího klíče hodnotu null, což vede k tyto důsledky:

| Název chování         | Vliv na závislé a podřízené v paměti | Vliv na závislé a podřízené databáze |
|:----------------------|:------------------------------------|:--------------------------------------|
| **CASCADE** (výchozí) | Entity se odstraní.                | Entity se odstraní.                  |
| **ClientSetNull**     | Vyvolá SaveChanges                  | Žádné                                  |
| **SetNull**           | Vyvolá SaveChanges                  | Vyvolá SaveChanges                    |
| **Omezení**          | Žádné                                | Žádné                                  |

V tabulkách výš *žádné* může mít za následek porušení omezení. Například pokud je odstraněn objekt nebo podřízený entity, ale chcete-li změnit cizí klíč závislé a podřízené nebyla provedena žádná akce, poté databázi bude pravděpodobně vyvolat na SaveChanges z důvodu narušení omezení pro cizí.

Na vysoké úrovni:
* Pokud máte entit, které nemůže existovat bez nadřazené a které EF abyste dbali pro odstranění podřízené objekty automaticky, a potom použijte *Cascade*.
  * Entity, která nemůže existovat bez nadřazený obvykle provést použít požadované vztahy, pro kterou *Cascade* je výchozí.
* Pokud máte entit, které může nebo nemusí mít nadřazenou položku, a které EF, která se postará o nulling out cizí klíč pro vás, a potom použijte *ClientSetNull*
  * Entit, které může existovat bez nadřazený obvykle provést pomocí volitelné vztahy, pro kterou *ClientSetNull* je výchozí.
  * Pokud chcete databázi pokusit také potřebný k šíření hodnoty null cizí klíče podřízené i když není načtený podřízené entity, pak použijte *SetNull*. Ale Upozorňujeme, že databáze musí podporovat to a konfigurace databáze jako to může mít za následek další omezení, která v praxi často nepraktické tuto možnost. To je důvod, proč *SetNull* není výchozí.
* Pokud nechcete, aby základní EF někdy automaticky odstranění entity nebo hodnota null se cizí klíč automaticky a pak použít *omezit*. Všimněte si, že to vyžaduje, aby váš kód synchronizaci podřízených entit a jejich hodnoty cizího klíče ručně jinak omezení výjimky bude vyvolána.

> [!NOTE]
> V EF Core, na rozdíl od EF6 kaskádových důsledky není dojít okamžitě, ale místo toho pouze když je volána metoda SaveChanges.

> [!NOTE]  
> **Změny v EF základní 2.0:** v předchozích verzích *omezit* by způsobilo volitelné vlastnosti cizího klíče v sledovaných závislé entity na hodnotu null a byla odstranit výchozí chování pro volitelné relace. V EF základní 2.0 *ClientSetNull* bylo zavedeno představují daná chování a aktivovala na výchozí hodnoty pro volitelné vztahy. Chování *omezit* byla upravena nikdy mít žádné vedlejší účinky na závislé entity.

## <a name="entity-deletion-examples"></a>Příklady odstranění entity

Následující kód je součástí [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , je možné stáhnout a spustit. Ukázka zobrazuje, co se stane pro každý odstranit chování pro volitelné a požadované relace při odstranění nadřazená entita.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Projděme každý variace pochopit, co se děje.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade s požadované nebo volitelné relace

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

* Blog je označena jako odstraněné
* Příspěvky původně zůstat Unchanged, protože není dojít cascades, dokud SaveChanges
* SaveChanges odešle odstranění závislé objekty nebo podřízených prvků (příspěvky) a pak hlavní nebo nadřízené (blog)
* Po uložení všechny entity jsou odpojené od teď z databáze byla odstraněna

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s požadovaná relace

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

* Blog je označena jako odstraněné
* Příspěvky původně zůstat Unchanged, protože není dojít cascades, dokud SaveChanges
* SaveChanges, pokusí se nastavit post cizího klíče na hodnotu null, ale to nezdaří, protože není Cizíklíč s možnou hodnotou Null

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s volitelný vztah

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

* Blog je označena jako odstraněné
* Příspěvky původně zůstat Unchanged, protože není dojít cascades, dokud SaveChanges
* SaveChanges pokusy nastaví Cizíklíč obě položky závislé na nebo podřízených prvků (příspěvky) na hodnotu null. před odstraněním hlavní nebo nadřízené (blog)
* Po uložení se odstraní objekt zabezpečení nebo nadřízené (blog), ale stále sledovány závislé objekty nebo podřízené objekty (příspěvky)
* Sledovaných závislé objekty nebo podřízené objekty (příspěvky) teď mít hodnotu null. hodnoty cizího klíče a odebrala se jejich odkaz na odstraněného objektu zabezpečení nebo nadřízené (blog)

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict s požadované nebo volitelné relace

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

* Blog je označena jako odstraněné
* Příspěvky původně zůstat Unchanged, protože není dojít cascades, dokud SaveChanges
* Vzhledem k tomu *omezit* informuje EF automaticky nastavit cizího klíče na hodnotu null, zůstane jinou hodnotu než null a SaveChanges vyvolá bez uložení

## <a name="delete-orphans-examples"></a>Odstranit příklady osamocené položky

Následující kód je součástí [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) který lze stáhnout spustit. Ukázka zobrazuje, co se stane pro každý odstranit chování pro volitelné a požadované relace při je vztah mezi nadřazenou nebo objekt zabezpečení a jeho podřízených objektů/dependents porušeno. V tomto příkladu je relace porušeno odebráním závislosti nebo podřízené objekty (příspěvky) z navigační vlastnosti kolekce na objekt zabezpečení nebo nadřízené (blog). Chování je však stejný Pokud ze závislých a podřízené odkaz na objekt zabezpečení nebo nadřízené je místo toho vynulovanými limitu.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Projděme každý variace pochopit, co se děje.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade s požadované nebo volitelné relace

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

* Příspěvky jsou označeny jako změněné, protože severing relace způsobilo Cizíklíč byla označena jako hodnotu null.
  * Pokud Cizíklíč není s možnou hodnotou Null, pak se skutečnou hodnotou nedojde ke změně i když je označen jako hodnotu null.
* SaveChanges odešle odstranění pro závislé objekty (příspěvky) podřízené objekty
* Po uložení položky závislé na nebo podřízené objekty (příspěvky) jsou odpojené od teď z databáze byla odstraněna

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s požadovaná relace

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

* Příspěvky jsou označeny jako změněné, protože severing relace způsobilo Cizíklíč byla označena jako hodnotu null.
  * Pokud Cizíklíč není s možnou hodnotou Null, pak se skutečnou hodnotou nedojde ke změně i když je označen jako hodnotu null.
* SaveChanges, pokusí se nastavit post cizího klíče na hodnotu null, ale to nezdaří, protože není Cizíklíč s možnou hodnotou Null

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s volitelný vztah

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

* Příspěvky jsou označeny jako změněné, protože severing relace způsobilo Cizíklíč byla označena jako hodnotu null.
  * Pokud Cizíklíč není s možnou hodnotou Null, pak se skutečnou hodnotou nedojde ke změně i když je označen jako hodnotu null.
* SaveChanges nastaví Cizíklíč obě položky závislé na nebo podřízených prvků (příspěvky) na hodnotu null.
* Po uložení závislé objekty nebo podřízené objekty (příspěvky) teď mít hodnotu null. hodnoty cizího klíče a odebrala se jejich odkaz na odstraněného objektu zabezpečení nebo nadřízené (blog)

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict s požadované nebo volitelné relace

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

* Příspěvky jsou označeny jako změněné, protože severing relace způsobilo Cizíklíč byla označena jako hodnotu null.
  * Pokud Cizíklíč není s možnou hodnotou Null, pak se skutečnou hodnotou nedojde ke změně i když je označen jako hodnotu null.
* Vzhledem k tomu *omezit* informuje EF automaticky nastavit cizího klíče na hodnotu null, zůstane jinou hodnotu než null a SaveChanges vyvolá bez uložení

## <a name="cascading-to-untracked-entities"></a>Kaskádových na Nesledované entity

Při volání *SaveChanges*, cascade odstranit pravidla se použijí pro všechny entity, které jsou sledovány kontextu. Toto je situaci ve všech příklady uvedené nahoře, který je důvod, proč SQL byl vygenerován odstranit objekt zabezpečení nebo nadřízené (blog) a všechny závislé objekty nebo podřízené objekty (příspěvky):

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Pokud pouze objekt je načteno – například když se dotazuje pro blog bez `Include(b => b.Posts)` zahrnout také příspěvcích – pak SaveChanges vygeneruje jenom SQL se odstranit objekt zabezpečení nebo nadřízené:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Závislé objekty nebo podřízené objekty (příspěvky) budou odstraněny, pouze pokud databáze obsahuje odpovídající cascade chování nakonfigurovaném. Pokud používáte EF k vytvoření databáze, bude toto chování cascade instalace pro vás.
