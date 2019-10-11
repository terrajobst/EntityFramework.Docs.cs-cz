---
title: Kaskádové odstranění – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: af86383bad52c87d2874fa4f8eb247a656601312
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182015"
---
# <a name="cascade-delete"></a>Kaskádové odstranění

Kaskádové odstraňování se často používá v terminologii databáze k popisu charakteristiky, která umožňuje odstranění řádku automaticky aktivovat odstranění souvisejících řádků. Úzce související pojem, který je pokrytý EF Core odstranění chování je automatické odstranění podřízené entity, pokud je relace vůči nadřazenému objektu nadřazená – to se obvykle označuje jako "odstranění osamocení".

EF Core implementuje několik různých chování při odstraňování a umožňuje konfiguraci chování při odstraňování jednotlivých vztahů. EF Core také implementuje konvence, které automaticky nakonfigurují užitečné výchozí chování při odstraňování každého vztahu na základě [požadované hodnoty vztahu](../modeling/relationships.md#required-and-optional-relationships).

## <a name="delete-behaviors"></a>Chování při odstranění
Chování při odstranění jsou definovaná v typu enumerátoru *DeleteBehavior* a dají se předat rozhraní API pro *odstranění* Fluent, které určuje, jestli se má odstranit objekt zabezpečení nebo nadřazená entita nebo závažnost vztahu k závislým/podřízeným entitám. mají vedlejší efekt na závislých nebo podřízených entitách.

Existují tři akce, které EF může provést při odstranění objektu zabezpečení nebo nadřazené entity nebo při jeho vztahu k podřízenému objektu:
* Podřízená/závislá je možné odstranit.
* Hodnoty cizího klíče dítěte mohou být nastaveny na hodnotu null.
* Podřízená položka zůstane beze změny.

> [!NOTE]  
> Chování při odstranění, které je nakonfigurované v modelu EF Core, se použije jenom v případě, že je primární entita Odstraněná pomocí EF Core a závislé entity se načtou do paměti (tj. pro sledované závislé položky). V databázi musí být nastavené odpovídající chování funkce Cascade, aby se zajistilo, že data, která nejsou sledována kontextem, jsou použita potřebná akce. Pokud k vytvoření databáze použijete EF Core, bude chování kaskádového nastavení pro vás.

U druhé akce výše nastavení hodnota cizího klíče na hodnotu null není platné, pokud cizí klíč nemůže mít hodnotu null. (Cizí klíč, který nemůže mít hodnotu null, je ekvivalentní k požadované relaci.) V těchto případech EF Core sleduje, že vlastnost cizího klíče byla označena jako null, dokud není volána metoda SaveChanges, přičemž v této době je vyvolána výjimka, protože změnu nelze trvale uložit do databáze. To je podobné jako získání porušení omezení z databáze.

Existují čtyři chování při odstranění, jak je uvedeno v následujících tabulkách.

### <a name="optional-relationships"></a>Volitelné relace
U volitelných vztahů (cizí klíč s možnou hodnotou null) _je_ možné uložit hodnotu cizího klíče s hodnotou null, která má za následek následující důsledky:

| Název chování               | Vliv na závislé nebo podřízené objekty v paměti    | Vliv na závislé nebo podřízené v databázi  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| **Nášejí**                 | Entity jsou odstraněny                   | Entity jsou odstraněny                   |
| **ClientSetNull** (výchozí) | Vlastnosti cizího klíče jsou nastaveny na hodnotu null. | Žádné                                   |
| **SetNull**                 | Vlastnosti cizího klíče jsou nastaveny na hodnotu null. | Vlastnosti cizího klíče jsou nastaveny na hodnotu null. |
| **Omezit**                | Žádné                                   | Žádné                                   |

### <a name="required-relationships"></a>Požadované relace
Pro požadované relace (cizí klíč, který nesmí mít hodnotu null), _není_ možné uložit hodnotu cizího klíče s hodnotou null, která má za následek následující důsledky:

| Název chování         | Vliv na závislé nebo podřízené objekty v paměti | Vliv na závislé nebo podřízené v databázi |
|:----------------------|:------------------------------------|:--------------------------------------|
| **Kaskádová** (výchozí) | Entity jsou odstraněny                | Entity jsou odstraněny                  |
| **ClientSetNull**     | SaveChanges vyvolá                  | Žádné                                  |
| **SetNull**           | SaveChanges vyvolá                  | SaveChanges vyvolá                    |
| **Omezit**          | Žádné                                | Žádné                                  |

V tabulkách uvedených výše může *žádný* z nich dojít k porušení omezení. Například pokud je odstraněna objekt hlavní/podřízená entita, ale není provedena žádná akce pro změnu cizího klíče závislého nebo podřízeného objektu, databáze pravděpodobně vyvolá operaci SaveChanges z důvodu narušení cizího omezení.

Na nejvyšší úrovni:
* Pokud máte entity, které nemůžou existovat bez nadřazeného objektu, a chcete, aby se EF postaral o automatické odstranění podřízených objektů, použijte *kaskády*.
  * Entity, které nemohou existovat bez nadřazeného objektu obvykle využívají požadované relace, pro které je výchozí hodnota *Cascade* .
* Pokud máte entity, které mohou nebo nemusí mít nadřazenou položku a chcete, aby EF postaral o vynulování cizího klíče za vás, pak použijte *ClientSetNull*
  * Entity, které mohou existovat bez nadřazeného objektu obvykle využívají volitelné vztahy, pro které je výchozí hodnota *ClientSetNull* .
  * Pokud chcete, aby se databáze také pokusila rozšířit hodnoty null na podřízené cizí klíče i v případě, že není načtena podřízená entita, použijte *SetNull*. Uvědomte si však, že databáze musí podporovat tuto databázi, a pokud ji nakonfigurujete takto, může to mít za následek i jiná omezení, což v praxi často tuto možnost způsobuje nepraktické. To je důvod, proč *SetNull* není výchozí.
* Pokud nechcete, aby EF Core nikdy odstranit entitu automaticky, nebo vynulování cizího klíče automaticky vyprší, použijte možnost *omezit*. Všimněte si, že to vyžaduje, aby váš kód zachoval podřízené entity a jejich hodnoty cizího klíče v synchronizaci ručně, jinak budou vyvolány výjimky omezení.

> [!NOTE]
> V EF Core na rozdíl od EF6 se kaskádové efekty okamžitě neprojeví, ale pouze v případě, že je volána metoda SaveChanges.

> [!NOTE]  
> **Změny v EF Core 2,0:** V předchozích verzích by *omezení* způsobilo, že volitelné vlastnosti cizího klíče ve sledovaných závislých entitách budou nastaveny na hodnotu null a bylo výchozím chováním při odstraňování volitelných vztahů. V EF Core 2,0 byla zavedena *ClientSetNull* , která představuje toto chování a stala se výchozími pro volitelné relace. Chování *omezení* bylo upraveno tak, aby na závislých entitách nikdy neměly žádné vedlejší účinky.

## <a name="entity-deletion-examples"></a>Příklady odstranění entit

Níže uvedený kód je součástí [ukázky](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) , kterou je možné stáhnout a spustit. Ukázka ukazuje, co se stane pro každé chování při odstraňování volitelných i požadovaných relací při odstranění nadřazené entity.

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Pojďme si projít každou variantou, abychom porozuměli tomu, co se děje.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior. Cascade s povinným nebo volitelným vztahem

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

* Blog je označený jako odstraněný.
* Příspěvky zpočátku zůstávají beze změny, protože kaskády neprobíhá, dokud SaveChanges neproběhne.
* SaveChanges odesílá odstranění pro závislé položky i podřízené objekty (příspěvky) a pak hlavní/nadřazený (blog).
* Po uložení se všechny entity odpojí od chvíle, kdy byly z databáze odstraněny.

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior. ClientSetNull nebo DeleteBehavior. SetNull s požadovanou relací

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
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* Blog je označený jako odstraněný.
* Příspěvky zpočátku zůstávají beze změny, protože kaskády neprobíhá, dokud SaveChanges neproběhne.
* Metoda SaveChanges se pokusí nastavit hodnotu post FK na null, ale to se nepovede, protože hodnota FK nemůže mít hodnotu null.

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior. ClientSetNull nebo DeleteBehavior. SetNull s volitelným vztahem

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
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* Blog je označený jako odstraněný.
* Příspěvky zpočátku zůstávají beze změny, protože kaskády neprobíhá, dokud SaveChanges neproběhne.
* Při pokusu o použití metody SaveChanges se před odstraněním objektu zabezpečení nebo nadřazené položky (blog) nastaví FK (post) na hodnotu null.
* Po uložení se objekt zabezpečení nebo nadřazený prvek (blog) odstraní, ale závislé objekty a podřízené položky (příspěvky) jsou pořád sledovány.
* Sledované závislé položky/podřízené položky (příspěvky) nyní mají hodnoty neprázdných CK a jejich odkaz na odstraněný objekt zabezpečení nebo nadřazený objekt (blog) byl odebrán.

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior. restrict s povinným nebo volitelným vztahem

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
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* Blog je označený jako odstraněný.
* Příspěvky zpočátku zůstávají beze změny, protože kaskády neprobíhá, dokud SaveChanges neproběhne.
* Vzhledem k tomu, že příkaz *restrict* instruuje EF k automatickému nastavení hodnoty FK na hodnotu null, zůstává nenulová a možnost SaveChanges vyvolá bez uložení.

## <a name="delete-orphans-examples"></a>Odstranit osamocené příklady

Níže uvedený kód je součástí [ukázky](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) , kterou je možné stáhnout a spustit. Ukázka ukazuje, co se stane pro každé chování při odstraňování volitelných i požadovaných vztahů, pokud je v relaci mezi nadřazeným objektem, hlavním prvkem a jeho podřízenými a závislými prvky vážně. V tomto příkladu je relace závažná odebráním závislých a podřízených prvků (příspěvků) z navigační vlastnosti kolekce na objektu hlavní nebo nadřazené (blog). Chování je však stejné, pokud je odkaz z závislého nebo podřízeného objektu na objekt zabezpečení nebo nadřazený je namísto hodnoty null.

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Pojďme si projít každou variantou, abychom porozuměli tomu, co se děje.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior. Cascade s povinným nebo volitelným vztahem

```console
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

* Příspěvky jsou označeny jako upravené, protože závažnost vztahu způsobila, že hodnota FK je označena jako null.
  * Pokud hodnota FK není null, skutečná hodnota se nemění, i když je označena jako null.
* SaveChanges odesílá odstranění pro závislé objekty/podřízené položky (příspěvky).
* Po uložení se závislé položky nebo podřízené položky (příspěvky) odpojí od okamžiku odstranění z databáze.

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior. ClientSetNull nebo DeleteBehavior. SetNull s požadovanou relací

```console
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

* Příspěvky jsou označeny jako upravené, protože závažnost vztahu způsobila, že hodnota FK je označena jako null.
  * Pokud hodnota FK není null, skutečná hodnota se nemění, i když je označena jako null.
* Metoda SaveChanges se pokusí nastavit hodnotu post FK na null, ale to se nepovede, protože hodnota FK nemůže mít hodnotu null.

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior. ClientSetNull nebo DeleteBehavior. SetNull s volitelným vztahem

```console
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

* Příspěvky jsou označeny jako upravené, protože závažnost vztahu způsobila, že hodnota FK je označena jako null.
  * Pokud hodnota FK není null, skutečná hodnota se nemění, i když je označena jako null.
* SaveChanges nastaví FK pro závislé objekty nebo podřízené objekty (post) na hodnotu null.
* Po uložení budou mít následníci/podřízené (příspěvky) nyní hodnoty neprázdných hodnot FK a jejich odkaz na odstraněný objekt zabezpečení nebo nadřazený objekt (blog) byl odebrán.

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior. restrict s povinným nebo volitelným vztahem

```console
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

* Příspěvky jsou označeny jako upravené, protože závažnost vztahu způsobila, že hodnota FK je označena jako null.
  * Pokud hodnota FK není null, skutečná hodnota se nemění, i když je označena jako null.
* Vzhledem k tomu, že příkaz *restrict* instruuje EF k automatickému nastavení hodnoty FK na hodnotu null, zůstává nenulová a možnost SaveChanges vyvolá bez uložení.

## <a name="cascading-to-untracked-entities"></a>Kaskádové k nesledovaným entitám

Při volání *metody SaveChanges*budou pravidla pro odstranění kaskádových pravidel použita u všech entit, které jsou sledovány v kontextu. Jedná se o situaci ve všech výše uvedených příkladech, což je důvod, proč byl vygenerován SQL, aby odstranil hlavní nebo nadřazený objekt (blog) a všechny závislé prvky/podřízené položky (příspěvky):

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Pokud je načtený pouze objekt zabezpečení (například když je proveden dotaz na blogu) bez `Include(b => b.Posts)` pro zahrnutí příspěvků, pak SaveChanges vygeneruje pouze SQL pro odstranění objektu zabezpečení nebo nadřazené položky:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Závislé objekty a podřízené položky (příspěvky) se odstraní pouze v případě, že má databáze nakonfigurované odpovídající chování kaskádového chování. Pokud k vytvoření databáze použijete EF, toto chování kaskádové instalace bude nastaveno za vás.
