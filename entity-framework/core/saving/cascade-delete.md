---
title: Odstranění – EF Core v kaskádě
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: 15b7e69676ef9aeb70121fcec404c34a17e5e2bb
ms.sourcegitcommit: 8d04a2ad98036f32ca70c77ce3040c5edb1cdf82
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/11/2018
ms.locfileid: "44384836"
---
# <a name="cascade-delete"></a>Kaskádové odstranění

Kaskádové odstranění se běžně používá v, řečeno terminologií databáze k popisu znakem, který umožňuje odstranění řádku pro automatickou aktivaci odstranění související řádky. Úzce související pojem také popsaná v EF Core odstranit chování je automatické odstranění podřízené entity, když jeho vztah k nadřazené bylo porušeno – to se běžně označuje jako "odstraňování osamocené položky".

EF Core implementuje několik různých odstranit chování a umožňuje konfigurovat chování odstranění jednotlivých vztahů. EF Core také implementuje vytváření názvů, který automaticky nakonfiguruje odstranit užitečné výchozí chování pro každou relaci na základě [requiredness relace](../modeling/relationships.md#required-and-optional-relationships).

## <a name="delete-behaviors"></a>Odstranit chování
Odstranit chování, které jsou definovány v *DeleteBehavior* enumerátor typu a mohou být předány *elementy OnDelete* fluent API pro ovládací prvek, zda odstranění objektu zabezpečení/nadřazená entita nebo severing z vztah k entity závislé/podřízený by měl mít vedlejší účinek v závislé/podřízené entity.

Existují tři akce, které EF můžete provést, když je odstraněn objekt zabezpečení/nadřazená entita nebo porušeno vztah podřízený:
* Podřízené rolích dependent je možné odstranit.
* Podřízené hodnoty cizího klíče můžete nastavit na hodnotu null
* Podřízené zůstane beze změny

> [!NOTE]  
> Odstranit chování nakonfigurovaném v modelu EF Core platí pouze při instančního objektu entity se odstraní pomocí EF Core a entity, které závislé jsou načtena do paměti (to znamená pro sledované závislosti). Odpovídající chování cascade musí být, že má instalační program v databázi pro zajištění dat, která není sledována kontextu použité potřebné akce. Pokud použijete k vytvoření databáze EF Core, bude toto chování cascade nastavení za vás.

Pro druhý výše uvedenou akci nastavení hodnoty cizího klíče na hodnotu null není platná, pokud není cizí klíč s možnou hodnotou Null. (Je ekvivalentní povinný vztah cizího klíče nepřipouštějící.) V těchto případech EF Core sleduje, že vlastnost cizího klíče byl označen jako null dokud se nazývá SaveChanges, kdy je vyvolána výjimka, protože změny nelze nastavit jako trvalý do databáze. Toto je podobný získávání porušení omezení z databáze.

Existují čtyři odstranit chování, jak je uvedeno v následujících tabulkách.

### <a name="optional-relationships"></a>Volitelné relace
Pro volitelné relace (s možnou hodnotou Null cizí klíč) ji _je_ možné uložit cizího klíče hodnotu null, což vede k následujících efektů:

| Název chování               | Vliv na závislé a podřízené v paměti    | Vliv na závislé/podřízené databáze  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| **Kaskádové**                 | Odstranění entit                   | Odstranění entit                   |
| **ClientSetNull** (výchozí) | Vlastnosti cizího klíče jsou nastaveny na hodnotu null | Žádné                                   |
| **SetNull**                 | Vlastnosti cizího klíče jsou nastaveny na hodnotu null | Vlastnosti cizího klíče jsou nastaveny na hodnotu null |
| **omezení**                | Žádné                                   | Žádné                                   |

### <a name="required-relationships"></a>Požadovaná relace
Požadovaná relace (neumožňující cizí klíč) je _není_ možné uložit cizího klíče hodnotu null, což vede k následujících efektů:

| Název chování         | Vliv na závislé a podřízené v paměti | Vliv na závislé/podřízené databáze |
|:----------------------|:------------------------------------|:--------------------------------------|
| **Kaskádové** (výchozí) | Odstranění entit                | Odstranění entit                  |
| **ClientSetNull**     | Vyvolá SaveChanges                  | Žádné                                  |
| **SetNull**           | Vyvolá SaveChanges                  | Vyvolá SaveChanges                    |
| **omezení**          | Žádné                                | Žádné                                  |

V tabulkách výše *žádný* může vést k narušení omezení. Například pokud se entita hlavní/podřízený se odstraní, ale chcete-li změnit cizí klíč závislé/podřízený nebyla provedena žádná akce, poté databázi pravděpodobně vyvolá na SaveChanges z důvodu narušení omezení pro cizí.

Na vysoké úrovni:
* Pokud máte entity, které nemohou existovat bez nadřazených a EF postará za odstranění podřízené objekty automaticky a pak použijte *Cascade*.
  * Entity, které nemohou existovat bez nadřazené obvykle provést pomocí požadované vztahů, pro kterou *Cascade* je výchozí nastavení.
* Pokud máte entity, které může nebo nemusí být nadřazenou položku a EF, která se stará o nulling si cizí klíč pro vás a pak použijte *ClientSetNull*
  * Entity, které mohou existovat bez nadřazené obvykle provést použít volitelný vztahů, pro kterou *ClientSetNull* je výchozí nastavení.
  * Pokud chcete databázi taky zkusit šíření hodnoty null k cizím klíčům podřízené i když není načten podřízené entity, pak použijte *SetNull*. Mějte však na paměti, že databáze musí podporovat to a konfigurace databáze tímto způsobem může způsobit další omezení, která v praxi často nepraktické tuto možnost. To je důvod, proč *SetNull* není výchozí.
* Pokud nechcete, aby EF Core pro nikdy automaticky odstranit entitu nebo hodnota null, cizí klíč automaticky, a následné použití *omezit*. Upozorňujeme, že to vyžaduje, aby váš kód synchronizaci podřízených entit a jejich hodnoty cizího klíče ručně jinak omezení výjimky budou vyvolány.

> [!NOTE]
> V EF Core, na rozdíl od EF6 kaskádové účinky neproběhne okamžitě, ale místo toho pouze když je volána metoda SaveChanges.

> [!NOTE]  
> **Změny v EF Core 2.0:** v předchozích verzích *omezit* by způsobilo volitelné vlastnosti cizího klíče v sledované závislé můžou být nastavena na hodnotu null a je výchozí chování pro volitelné relací při odstranění. V EF Core 2.0 *ClientSetNull* byl zaveden k reprezentaci tohoto chování a začal na výchozí hodnoty pro nepovinný vztahy. Chování *omezit* byla upravena nikdy mít žádné vedlejší účinky na závislých položek.

## <a name="entity-deletion-examples"></a>Příklady odstranění entity

Následující kód je součástí [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , který je možné stáhnout a spustit. Vzorek ukazuje, co se stane pro každou chování při odstraňování u povinných a volitelných relací při odstranění nadřazená entita.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Projděme si každý variace pochopit, co se děje.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade požadované nebo volitelné vztah

```
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

* Blog je označený jako odstraněný
* Příspěvky zpočátku zůstat Unchanged, protože cascades neproběhne až do SaveChanges
* SaveChanges odešle odstraní závislé položky a podřízené položky (příspěvků) a potom objekt zabezpečení/nadřazený (blog)
* Po uložení se všechny entity odpojit, protože nyní z databáze byla odstraněna

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s povinný vztah

```
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

* Blog je označený jako odstraněný
* Příspěvky zpočátku zůstat Unchanged, protože cascades neproběhne až do SaveChanges
* SaveChanges, pokusí se nastavit příspěvek cizího klíče na hodnotu null, ale to se nezdaří, protože není FK s možnou hodnotou Null

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s volitelný vztah

```
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

* Blog je označený jako odstraněný
* Příspěvky zpočátku zůstat Unchanged, protože cascades neproběhne až do SaveChanges
* Pokusy o SaveChanges FK obě položky závislé na/podřízené prvky (příspěvků) nastaví na hodnotu null před odstraněním objektu zabezpečení/nadřazený (blog)
* Po uložení objektu zabezpečení/nadřazený (blog) je odstraněn, ale závislé položky a podřízené objekty (příspěvků) jsou stále sledována.
* Sledované závislé položky a podřízené objekty (příspěvků) teď obsahují hodnoty null cizího klíče a jejich odkaz na odstraněný objekt zabezpečení/nadřazený (blog) byla odebrána.

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict požadované nebo volitelné vztah

```
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

* Blog je označený jako odstraněný
* Příspěvky zpočátku zůstat Unchanged, protože cascades neproběhne až do SaveChanges
* Protože *omezit* říká EF automaticky nastavit cizího klíče na hodnotu null, zůstane jinou hodnotu než null a SaveChanges vyvolá bez uložení

## <a name="delete-orphans-examples"></a>Odstranit příklady osamocené položky

Následující kód je součástí [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , který je možné stáhnout a spustit. Vzorek ukazuje, co se stane pro každou chování při odstraňování u povinných a volitelných relací při porušeno vztah nadřazeného/instanční objekt a jeho podřízené objekty/dependents. V tomto příkladu je vztah porušeno odebráním závislé položky a podřízené objekty (příspěvků) z navigační vlastnost kolekce na objekt zabezpečení/nadřazený (blog). Chování je však stejný Pokud odkaz z závislé/podřízený na objekt zabezpečení/nadřazený prvek je místo toho vynulovanými navýšení kapacity.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Projděme si každý variace pochopit, co se děje.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade požadované nebo volitelné vztah

```
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

* Příspěvky jsou označeny jako změněno, protože severing vztah způsobily FK být označený jako null
  * Pokud FK není s možnou hodnotou Null, pak skutečnou hodnotu nedojde ke změně i v případě, že je označena jako hodnota null
* SaveChanges odešle odstraní pro závislé položky a podřízené položky (příspěvků)
* Po uložení, jsou závislé položky a podřízené objekty (příspěvků) odpojit, protože nyní z databáze byla odstraněna

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s povinný vztah

```
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

* Příspěvky jsou označeny jako změněno, protože severing vztah způsobily FK být označený jako null
  * Pokud FK není s možnou hodnotou Null, pak skutečnou hodnotu nedojde ke změně i v případě, že je označena jako hodnota null
* SaveChanges, pokusí se nastavit příspěvek cizího klíče na hodnotu null, ale to se nezdaří, protože není FK s možnou hodnotou Null

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull nebo DeleteBehavior.SetNull s volitelný vztah

```
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

* Příspěvky jsou označeny jako změněno, protože severing vztah způsobily FK být označený jako null
  * Pokud FK není s možnou hodnotou Null, pak skutečnou hodnotu nedojde ke změně i v případě, že je označena jako hodnota null
* SaveChanges FK obě položky závislé na/podřízené prvky (příspěvků) nastaví na hodnotu null
* Po uložení, závislé položky a podřízené objekty (příspěvků) teď obsahují hodnoty null cizího klíče a jejich odkaz na odstraněný objekt zabezpečení/nadřazený (blog) byla odebrána.

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict požadované nebo volitelné vztah

```
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

* Příspěvky jsou označeny jako změněno, protože severing vztah způsobily FK být označený jako null
  * Pokud FK není s možnou hodnotou Null, pak skutečnou hodnotu nedojde ke změně i v případě, že je označena jako hodnota null
* Protože *omezit* říká EF automaticky nastavit cizího klíče na hodnotu null, zůstane jinou hodnotu než null a SaveChanges vyvolá bez uložení

## <a name="cascading-to-untracked-entities"></a>Kaskádová šablona Nesledované entity

Při volání *SaveChanges*, kaskádové odstranění pravidla se použijí pro všechny entity, které jsou sledovány kontextu. Toto je situace, ve všech příkladů uvedených výše, což je důvod, proč SQL se vygeneroval se odstranit objekt zabezpečení/nadřazený (blog) a všechny závislé položky a podřízené objekty (příspěvků):

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Pokud pouze pro objekt zabezpečení je načteno – například když se provede dotaz pro blog bez `Include(b => b.Posts)` Pokud chcete také zahrnout příspěvky – potom SaveChanges vygeneruje jenom SQL se odstranit objekt zabezpečení/nadřazený prvek:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Závislé položky a podřízené objekty (příspěvků) budou odstraněny, pouze pokud databáze nemá odpovídající chování cascade nakonfigurovaném. Pokud použijete k vytvoření databáze EF, bude toto chování cascade nastavení za vás.
