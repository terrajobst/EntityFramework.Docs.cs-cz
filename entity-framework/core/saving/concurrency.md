---
title: Zpracování konfliktů souběžnosti - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 288d9c6fced5ebbaa2c366248c68547502c3698e
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/05/2018
---
# <a name="handling-concurrency-conflicts"></a>Zpracování konfliktů souběžnosti

> [!NOTE]
> Tato stránka dokumenty fungování souběžnosti v EF jádra a způsobu řešení konfliktů souběžnosti ve vaší aplikaci. V tématu [tokenů souběžnosti](xref:core/modeling/concurrency) podrobnosti o tom, jak nakonfigurovat tokenů souběžnosti v modelu.

> [!TIP]
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) na Githubu.

_Databáze souběžnosti_ odkazuje na situace, ve kterých více procesů nebo uživatelé přístup, nebo změňte stejná data v databázi ve stejnou dobu. _Kontrola souběžnosti_ odkazuje na konkrétní mechanismy, které se používá k zajištění konzistence dat v přítomnost souběžných změny.

Implementuje EF základní _optimistické řízení souběžného_, což znamená, že se vám umožní více procesů nebo uživatelé provádět změny nezávisle bez režie synchronizace nebo uzamčení. V ideálním případě tyto změny nebude docházet k vzájemnému rušení a proto nebudou úspěšné. V nejhorším scénářem případu dvě nebo více procesů se pokusí provést konfliktní změny a pouze jeden z nich by měl být úspěšné.

## <a name="how-concurrency-control-works-in-ef-core"></a>Jak funguje řízení souběžného zpracování EF jádra

Vlastnosti nakonfigurovaný jako tokenů souběžnosti slouží k implementaci optimistické řízení souběžného: vždy, když se provádí operace aktualizace nebo odstranění během `SaveChanges`, hodnota tokenu souběžnosti v databázi se porovná s původní Hodnota číst EF jádra.

- Pokud se hodnoty shodují, můžete dokončit operaci.
- Pokud se hodnoty neshodují, EF základní předpokládá, že jiný uživatel provedl konfliktní operace a zruší aktuální transakci.

V případě, když jiného uživatele byla provedena operace, které jsou v konfliktu s aktuální operace se označuje jako _souběžnosti konflikt_.

Zprostředkovatelé databáze jsou zodpovědní za implementaci porovnání hodnot token souběžnosti.

U relačních databází EF základní zahrnuje kontrolu hodnoty tokenu souběžnosti v `WHERE` klauzule libovolného `UPDATE` nebo `DELETE` příkazy. Po provedení příkazy, přečte EF základní počet řádků, které situace měla vliv na.

Pokud jsou vliv na žádné řádky, je zjištěn konflikt souběžnosti a vyvolá EF základní `DbUpdateConcurrencyException`.

Například může chceme konfigurovat `LastName` na `Person` být token souběžnosti. Potom všechny operace aktualizace na osoba bude obsahovat souběžnosti změnami `WHERE` klauzule:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Řešení konfliktů souběžnosti

Pokračovat na předchozí příklad, pokud se jeden uživatel se pokusí uložit některé změny `Person`, ale již změnil jiný uživatel `LastName` k výjimce.

V tomto okamžiku může aplikace jednoduše informujte uživatele, že aktualizace nebyla úspěšná kvůli konfliktu změn a přesunutí na. Ale může být žádoucí výzvy, ujistěte se, že tento záznam stále představuje stejná skutečná osoba a operaci opakujte.

Tento proces je příkladem _vyřešení konfliktu souběžnosti_.

Řešení konfliktů souběžnosti zahrnuje slučování čekajících změn z aktuální `DbContext` s hodnotami v databázi. Jaké hodnoty nesloučí budou lišit v závislosti na aplikaci a může přesměrováni vstup uživatele.

**Existují tři sady hodnot, které vám pomůžou vyřešit konflikt souběžnosti:**

* **Aktuální hodnoty** jsou hodnoty, které aplikace při pokusu o zápis do databáze.

* **Původní hodnoty** jsou hodnoty, které byly původně načteny z databáze, před jakoukoli úpravou byly provedeny.

* **Databáze hodnoty** jsou hodnoty, které jsou aktuálně uloženy v databázi.

Obecné postup pro zpracování konfliktů souběžnosti je:

1. Catch – `DbUpdateConcurrencyException` během `SaveChanges`.
2. Použití `DbUpdateConcurrencyException.Entries` připravit novou sadu změn pro ovlivněné entity.
3. Obnovte původní hodnoty tokenu souběžnosti tak, aby odrážela aktuálních hodnot v databázi.
4. Proces opakujte, dokud dojít ke konfliktům.

V následujícím příkladu `Person.FirstName` a `Person.LastName` se instalační program jako tokenů souběžnosti. Je `// TODO:` komentář do umístění, kde zahrnete určitou logiku aplikace zvolte hodnotu uložit.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
