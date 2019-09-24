---
title: Zpracování konfliktů souběžnosti – EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: 4d6ff24e58caa0b228e9c1e4313beda78d1025fc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197828"
---
# <a name="handling-concurrency-conflicts"></a>Zpracování konfliktů souběžnosti

> [!NOTE]
> Tato stránka popisuje, jak souběžnost funguje v EF Core a jak zpracovávat konflikty souběžnosti ve vaší aplikaci. Podrobnosti o tom, jak konfigurovat tokeny souběžnosti v modelu, najdete v tématu [tokeny souběžnosti](xref:core/modeling/concurrency) .

> [!TIP]
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) na Githubu.

_Souběžnost databáze_ odkazuje na situace, ve kterých více procesů nebo uživatelů přistupuje nebo mění stejná data v databázi ve stejnou dobu. _Řízení souběžnosti_ odkazuje na konkrétní mechanismy, které slouží k zajištění konzistence dat v přítomnosti souběžných změn.

EF Core implementuje _optimistické řízení souběžnosti_, což znamená, že umožní více procesů nebo uživatelům provádět změny nezávisle bez režie synchronizace nebo uzamčení. V ideálním případě tyto změny nebudou navzájem ovlivněny, a proto budou moci být úspěšné. V nejhorším případě se dva nebo více procesů pokusí provést konfliktní změny a pouze jeden z nich by měl být úspěšný.

## <a name="how-concurrency-control-works-in-ef-core"></a>Jak funguje řízení souběžnosti v EF Core

Vlastnosti konfigurované jako tokeny souběžnosti se používají k implementaci optimistického řízení souběžnosti: kdykoli se provede `SaveChanges`operace Update nebo DELETE, hodnota tokenu souběžnosti v databázi se porovná s původní hodnotou. hodnota čtená EF Core.

- Pokud se hodnoty shodují, operace může být dokončena.
- Pokud se hodnoty neshodují, EF Core předpokládá, že jiný uživatel provedl konfliktní operaci a přeruší aktuální transakci.

Situace, kdy jiný uživatel provedl operaci, která je v konfliktu s aktuální operací, se označuje jako _konflikt souběžnosti_.

Poskytovatelé databáze zodpovídají za implementaci porovnání hodnot tokenu souběžnosti.

V relačních databázích EF Core obsahuje kontrolu hodnoty tokenu souběžnosti v `WHERE` klauzuli libovolných `UPDATE` příkazů nebo `DELETE` . Po provedení příkazů EF Core přečte počet ovlivněných řádků.

Pokud nejsou ovlivněny žádné řádky, je zjištěn konflikt souběžnosti a EF Core vyvolá `DbUpdateConcurrencyException`.

Můžeme například chtít nakonfigurovat `LastName` `Person` , aby byl token souběžnosti. Pak všechny operace aktualizace na osobu budou zahrnovat kontrolu souběžnosti v `WHERE` klauzuli:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Řešení konfliktů souběžnosti

Pokud budete pokračovat s předchozím příkladem, pokud se jeden uživatel pokusí uložit některé změny do `Person`, ale jiný uživatel je již `LastName`změnil, bude vyvolána výjimka.

V tomto okamžiku může aplikace jednoduše informovat uživatele o tom, že aktualizace nebyla úspěšná kvůli konfliktním změnám a přesunutím. Může být ale žádoucí vyzvat uživatele, aby tento záznam stále představoval stejnou skutečnou osobu a operaci zkuste zopakovat.

Tento proces je příkladem _řešení konfliktu souběžnosti_.

Vyřešení konfliktu souběžnosti zahrnuje sloučení nedokončených změn z aktuální `DbContext` hodnoty s hodnotami v databázi. Hodnoty, které se sloučí, se budou lišit v závislosti na aplikaci a můžou být směrovány uživatelským vstupem.

**Existují tři sady hodnot, které umožňují vyřešit konflikt souběžnosti:**

* **Aktuální hodnoty** jsou hodnoty, které aplikace pokoušela zapisovat do databáze.

* **Původní hodnoty** jsou hodnoty, které byly původně načteny z databáze před provedením jakýchkoli úprav.

* **Hodnoty databáze** jsou hodnoty, které jsou aktuálně uloženy v databázi.

Obecný přístup ke zpracování konfliktů souběžnosti je:

1. Zachytit `DbUpdateConcurrencyException` během `SaveChanges`.
2. Slouží `DbUpdateConcurrencyException.Entries` k přípravě nové sady změn pro ovlivněné entity.
3. Aktualizuje původní hodnoty tokenu souběžnosti tak, aby odrážely aktuální hodnoty v databázi.
4. Opakujte tento postup, dokud nedojde k žádnému konfliktu.

V následujícím příkladu `Person.FirstName` a `Person.LastName` jsou nastavení jako tokeny souběžnosti. K dispozici `// TODO:` je komentář v umístění, kam zahrnete logiku specifickou pro aplikaci pro výběr hodnoty, která se má uložit.

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
