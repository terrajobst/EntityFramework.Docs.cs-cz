---
title: Zpracování konfliktů souběžnosti – základní ef
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: a1d1a5a11d482f9104691aa3c072dbd1c548e9f1
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417588"
---
# <a name="handling-concurrency-conflicts"></a>Zpracování konfliktů souběžnosti

> [!NOTE]
> Tato stránka dokumentuje, jak funguje souběžnost v EF Core a jak zpracovat konflikty souběžnosti ve vaší aplikaci. Podrobnosti o konfiguraci tokenů souběžnosti ve vašem modelu najdete v tématu [Tokeny souběžnosti.](xref:core/modeling/concurrency)

> [!TIP]
> Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) můžete zobrazit na GitHubu.

_Souběžnost databáze_ odkazuje na situace, ve kterých více procesů nebo uživatelů přístup nebo změnit stejná data v databázi ve stejnou dobu. _Řízení souběžnosti_ odkazuje na konkrétní mechanismy používané k zajištění konzistence dat v přítomnosti souběžných změn.

EF Core implementuje _optimistické řízení souběžnosti_, což znamená, že umožní více procesů nebo uživatelů provádět změny nezávisle bez režie synchronizace nebo uzamčení. V ideální situaci, tyto změny nebudou zasahovat do sebe, a proto budou moci uspět. V nejhorším případě se dva nebo více procesů pokusí provést konfliktní změny a pouze jeden z nich by měl být úspěšný.

## <a name="how-concurrency-control-works-in-ef-core"></a>Jak funguje řízení souběžnosti v ef core

Vlastnosti nakonfigurované jako tokeny souběžnosti se používají k implementaci `SaveChanges`optimistické řízení souběžnosti: při každém provedení operace aktualizace nebo odstranění během , hodnota tokenu souběžnosti v databázi je porovnána s původní hodnotou přečtenou EF Core.

- Pokud se hodnoty shodují, operace může být dokončena.
- Pokud se hodnoty neshodují, EF Core předpokládá, že jiný uživatel provedl konfliktní operaci a přeruší aktuální transakci.

Situace, kdy jiný uživatel provedl operaci, která je v konfliktu s aktuální operací, se označuje jako _konflikt souběžnosti_.

Poskytovatelé databáze jsou zodpovědní za implementaci porovnání hodnot tokenu souběžnosti.

Na relačních databázích EF Core obsahuje kontrolu hodnoty `WHERE` tokenu `UPDATE` souběžnosti v klauzuli any nebo `DELETE` statements. Po provedení příkazy EF Core přečte počet řádků, které byly ovlivněny.

Pokud nejsou ovlivněny žádné řádky, je zjištěn konflikt souběžnosti a EF Core vyvolá `DbUpdateConcurrencyException`.

Například můžeme chtít `LastName` nakonfigurovat `Person` na token souběžnosti. Pak všechny operace aktualizace na osobu bude `WHERE` zahrnovat kontrolu souběžnosti v klauzuli:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Řešení konfliktů souběžnosti

Pokračování v předchozím příkladu, pokud jeden uživatel pokusí `Person`uložit některé změny , `LastName`ale jiný uživatel již změnil , pak bude vyvolána výjimka.

V tomto okamžiku aplikace může jednoduše informovat uživatele, že aktualizace nebyla úspěšná z důvodu konfliktních změn a přejít. Ale může být žádoucí vyzvat uživatele, aby zajistily, že tento záznam stále představuje stejnou skutečnou osobu a opakovat operaci.

Tento proces je příkladem _řešení konfliktu souběžnosti_.

Řešení konfliktu souběžnosti zahrnuje sloučení čekající změny z `DbContext` aktuální s hodnotami v databázi. Jaké hodnoty se sloučí se bude lišit v závislosti na aplikaci a mohou být řízeny vstupem uživatele.

**K dispozici jsou tři sady hodnot, které pomáhají vyřešit konflikt souběžnosti:**

- **Aktuální hodnoty** jsou hodnoty, které se aplikace pokoušela zapsat do databáze.
- **Původní hodnoty** jsou hodnoty, které byly původně načteny z databáze, před všechny úpravy byly provedeny.
- **Hodnoty databáze** jsou hodnoty aktuálně uložené v databázi.

Obecný přístup ke zpracování konfliktů souběžnosti je:

1. Úlovek `DbUpdateConcurrencyException` `SaveChanges`během .
2. Slouží `DbUpdateConcurrencyException.Entries` k přípravě nové sady změn pro ovlivněné entity.
3. Aktualizujte původní hodnoty tokenu souběžnosti tak, aby odrážely aktuální hodnoty v databázi.
4. Opakujte proces, dokud nedojde ke konfliktům.

V následujícím `Person.FirstName` příkladu `Person.LastName` a jsou nastaveny jako tokeny souběžnosti. V umístění `// TODO:` je poznámka, kde zahrnete logiku specifickou pro aplikaci a zvolte hodnotu, která má být uložena.

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
