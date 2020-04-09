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
# <a name="cascade-delete"></a>Kaskádové odstranění

Kaskádové odstranění se běžně používá v databázové terminologii k popisu charakteristiky, která umožňuje odstranění řádku automaticky spustit odstranění souvisejících řádků. Úzce související koncept, který se také vztahuje chování odstranění jádra EF, je automatické odstranění podřízené entity, když byl přerušen jeho vztah k nadřazené entitě – to to je obecně známé jako "odstranění sirotků".

EF Core implementuje několik různých chování odstranění a umožňuje konfiguraci chování odstranění jednotlivých relací. EF Core také implementuje konvence, které automaticky konfigurují užitečné výchozí chování odstranění pro každý vztah na základě [požadované relace](../modeling/relationships.md#required-and-optional-relationships).

## <a name="delete-behaviors"></a>Odstranit chování

Odstranění chování jsou definovány v *DeleteBehavior* enumerator typu a mohou být předány *OnDelete* plynulý rozhraní API řídit, zda odstranění hlavní/nadřazené entity nebo přerušení vztahu závislé/podřízené entity by měl mít vedlejší účinek na závislé/podřízené entity.

Existují tři akce EF může provést při odstranění hlavní/nadřazené entity nebo vztah k podřízené mu je oddělen:

* Podřízenou/závislou osobu lze odstranit.
* Hodnoty cizího klíče dítěte lze nastavit na hodnotu null
* Dítě zůstává nezměněno

> [!NOTE]  
> Chování odstranění nakonfigurované v modelu EF Core se použije pouze v případě, že je hlavní entita odstraněna pomocí EF Core a závislé entity jsou načteny do paměti (to znamená pro sledované závislé osoby). Odpovídající kaskádové chování musí být nastaveno v databázi, aby bylo zajištěno, že data, která nejsou sledována kontextem, mají potřebnou akci. Pokud k vytvoření databáze použijete EF Core, bude toto kaskádové chování nastaveno pro vás.

Pro druhou akci výše nastavení hodnoty cizího klíče na hodnotu null není platné, pokud cizí klíč nelze stanovit hodnotu null. (Cizí klíč, který neuplatní, je ekvivalentní požadovanému vztahu.) V těchto případech EF Core sleduje, že vlastnost cizího klíče byla označena jako null, dokud SaveChanges je volána, kdy je vyvolána výjimka, protože změna nemůže být trvalé do databáze. To je podobné získání porušení omezení z databáze.

Existují čtyři chování odstranění, jak je uvedeno v následujících tabulkách.

### <a name="optional-relationships"></a>Volitelné vztahy

Pro volitelné vztahy (cizí klíč s možnou hodnotou s možnou hodnotou s platností, kterou lze ukládat za cenu null), _je_ možné uložit hodnotu cizího klíče null, což má za následek následující účinky:

| Název chování               | Vliv na závislé/dítě v paměti    | Vliv na závislé/podřízené v databázi  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| **Kaskády**                 | Entity jsou odstraněny.                   | Entity jsou odstraněny.                   |
| **ClientSetNull** (výchozí) | Vlastnosti cizího klíče jsou nastaveny na hodnotu null. | Žádný                                   |
| **Nastavit hodnotu Null**                 | Vlastnosti cizího klíče jsou nastaveny na hodnotu null. | Vlastnosti cizího klíče jsou nastaveny na hodnotu null. |
| **Omezit**                | Žádný                                   | Žádný                                   |

### <a name="required-relationships"></a>Požadované vztahy

Pro požadované vztahy (cizí klíč, který nelze utaji) _není_ možné uložit hodnotu cizího klíče null, což má za následek následující účinky:

| Název chování         | Vliv na závislé/dítě v paměti | Vliv na závislé/podřízené v databázi |
|:----------------------|:------------------------------------|:--------------------------------------|
| **Kaskáda** (výchozí) | Entity jsou odstraněny.                | Entity jsou odstraněny.                  |
| **Hodnota ClientSetNull**     | SaveChanges hází                  | Žádný                                  |
| **Nastavit hodnotu Null**           | SaveChanges hází                  | SaveChanges hází                    |
| **Omezit**          | Žádný                                | Žádný                                  |

Ve výše uvedených tabulkách *none* může mít za následek porušení omezení. Například pokud objekt zabezpečení/podřízená entita je odstraněn, ale žádná akce je přijata ke změně cizího klíče závislé/podřízené, pak databáze bude pravděpodobně vyvolat SaveChanges z důvodu narušení cizí omezení.

Na vysoké úrovni:

* Pokud máte entity, které nemohou existovat bez nadřazené položky, a chcete, aby ef dbát na odstranění podřízených automaticky, pak použijte *Cascade*.
  * Entity, které nemohou existovat bez nadřazeného objektu, obvykle využívají požadované vztahy, pro které je *výchozí hodnota.*
* Pokud máte entity, které mohou nebo nemusí mít nadřazený objekt, a chcete, aby ef postarat o zrušení cizího klíče pro vás, použijte *ClientSetNull*
  * Entity, které mohou existovat bez nadřazeného objektu, obvykle využívají volitelné vztahy, pro které je výchozí *hodnota ClientSetNull.*
  * Pokud chcete, aby databáze také pokusit se šířit hodnoty null na podřízené cizí klíče i v případě, že podřízená entita není načtena, použijte *SetNull*. Všimněte si však, že databáze musí podporovat toto a konfigurace databáze, jako je tento může mít za následek další omezení, která v praxi často umožňuje tuto možnost nepraktické. To je důvod, proč *SetNull* není výchozí.
* Pokud nechcete, aby EF Core někdy automaticky odstranil entitu nebo automaticky odstranil cizí klíč, použijte *možnost Omezit*. Všimněte si, že to vyžaduje, aby váš kód zachovat podřízené entity a jejich hodnoty cizího klíče v synchronizaci ručně jinak omezení výjimky budou vyvolány.

> [!NOTE]
> V EF Core, na rozdíl od EF6 kaskádové efekty nedojde okamžitě, ale místo toho pouze při SaveChanges je volána.

> [!NOTE]  
> **Změny v EF Core 2.0:** V předchozích verzích *restrict* by způsobit volitelné cizí klíč vlastnosti v sledované závislé entity, které mají být nastaveny na hodnotu null a byl výchozí chování odstranění pro volitelné vztahy. V EF Core 2.0 *ClientSetNull* byla zavedena reprezentovat toto chování a stal se výchozí pro volitelné vztahy. Chování *Restrict* byla upravena tak, aby nikdy žádné vedlejší účinky na závislé entity.

## <a name="entity-deletion-examples"></a>Příklady odstranění entity

Níže uvedený kód je součástí [ukázky,](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) kterou lze stáhnout a spustit. Ukázka ukazuje, co se stane pro každé chování odstranění pro volitelné i požadované vztahy při odstranění nadřazené entity.

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Pojďme projít každou variantu pochopit, co se děje.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade s povinnou nebo volitelnou relace

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

* Blog je označen jako Odstraněný
* Příspěvky zpočátku zůstávají beze změny, protože kaskády se nedějí, dokud SaveChanges
* SaveChanges odešle odstraní pro oba závislé osoby /podřízené příspěvky (příspěvky) a pak hlavní /nadřazený (blog)
* Po uložení jsou všechny entity odpojeny, protože byly nyní odstraněny z databáze.

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s požadovaným vztahem

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

* Blog je označen jako Odstraněný
* Příspěvky zpočátku zůstávají beze změny, protože kaskády se nedějí, dokud SaveChanges
* SaveChanges pokusí nastavit post FK na hodnotu null, ale to se nezdaří, protože FK není nullable

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s volitelným vztahem

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

* Blog je označen jako Odstraněný
* Příspěvky zpočátku zůstávají beze změny, protože kaskády se nedějí, dokud SaveChanges
* SaveChanges pokusy nastaví FK obou závislých /podřízených (příspěvky) na null před odstraněním jistiny/nadřazené (blog)
* Po uložení se odstraní jistina/nadřazená položka (blog), ale závislé osoby/podřízené položky (příspěvky) jsou stále sledovány.
* Sledované závislé osoby /podřízené položky (příspěvky) mají nyní nulové hodnoty FK a jejich odkaz na odstraněný hlavní/nadřazený (blog) byl odstraněn

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict s povinnou nebo volitelnou vztah

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

* Blog je označen jako Odstraněný
* Příspěvky zpočátku zůstávají beze změny, protože kaskády se nedějí, dokud SaveChanges
* Vzhledem k *tomu, že funkce Omezit* říká EF, aby automaticky nenastavila hodnotu FK na hodnotu null, zůstává nenullová a saveChanges vyvolá bez uložení

## <a name="delete-orphans-examples"></a>Odstranit příklady osamocené

Níže uvedený kód je součástí [ukázky,](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) kterou lze stáhnout a spustit. Ukázka ukazuje, co se stane pro každé chování odstranění pro volitelné i požadované vztahy při přerušeném vztahu mezi nadřazeným/primárním objektem a jeho podřízenými/závislými položkami. V tomto příkladu je vztah přerušen odebráním závislé osoby/podřízené položky (příspěvky) z kolekce navigační vlastnost na hlavní nebo nadřazený (blog). Chování je však stejné, pokud je odkaz z dependent/child na principal/parent zrušen.

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Pojďme projít každou variantu pochopit, co se děje.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade s povinnou nebo volitelnou relace

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

* Příspěvky jsou označeny jako Změněno, protože přerušení vztahu způsobilo, že FK byl označen jako null
  * Pokud FK nelze zrušit, skutečná hodnota se nezmění, i když je označena jako null
* SaveChanges odešle odstraní pro závislé osoby /podřízené příspěvky (příspěvky)
* Po uložení jsou závislé osoby/podřízené položky (příspěvky) odpojeny, protože byly nyní odstraněny z databáze

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s požadovaným vztahem

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

* Příspěvky jsou označeny jako Změněno, protože přerušení vztahu způsobilo, že FK byl označen jako null
  * Pokud FK nelze zrušit, skutečná hodnota se nezmění, i když je označena jako null
* SaveChanges pokusí nastavit post FK na hodnotu null, ale to se nezdaří, protože FK není nullable

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s volitelným vztahem

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

* Příspěvky jsou označeny jako Změněno, protože přerušení vztahu způsobilo, že FK byl označen jako null
  * Pokud FK nelze zrušit, skutečná hodnota se nezmění, i když je označena jako null
* SaveChanges nastaví FK obou závislých osob /podřízených (příspěvky) na hodnotu null
* Po uložení mají vyživovaní/podřízené osoby (příspěvky) nyní nulové hodnoty FK a jejich odkaz na odstraněný hlavní/nadřazený (blog) byl odstraněn

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict s povinnou nebo volitelnou vztah

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

* Příspěvky jsou označeny jako Změněno, protože přerušení vztahu způsobilo, že FK byl označen jako null
  * Pokud FK nelze zrušit, skutečná hodnota se nezmění, i když je označena jako null
* Vzhledem k *tomu, že funkce Omezit* říká EF, aby automaticky nenastavila hodnotu FK na hodnotu null, zůstává nenullová a saveChanges vyvolá bez uložení

## <a name="cascading-to-untracked-entities"></a>Kaskádové k nesledovaným entitám

Při volání *SaveChanges*, kaskády odstranění pravidla budou použity pro všechny entity, které jsou sledovány kontextem. To je situace ve všech výše uvedených příkladech, což je důvod, proč sql byl generován odstranit jak hlavní / rodič (blog) a všechny závislé osoby / podřízené příspěvky):

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Pokud je načten pouze objekt zabezpečení – například při dotazu pro blog bez `Include(b => b.Posts)` zahrnout také příspěvky--pak SaveChanges bude generovat pouze SQL odstranit hlavní nebo nadřazený:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Závislé položky/podřízené položky (příspěvky) budou odstraněny pouze v případě, že databáze má odpovídající kaskádové chování nakonfigurováno. Pokud k vytvoření databáze použijete EF, bude toto kaskádové chování nastaveno za vás.
