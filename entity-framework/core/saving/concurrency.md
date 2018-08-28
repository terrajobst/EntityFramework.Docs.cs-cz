---
title: Zpracování konfliktů souběžnosti – EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: e050b17bfa31a4785161c700bc0355e83162b405
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993109"
---
# <a name="handling-concurrency-conflicts"></a>Zpracování konfliktů souběžnosti

> [!NOTE]
> Tato stránka dokumenty fungování souběžnosti v EF Core a způsob zpracování konfliktů souběžnosti v aplikaci. Zobrazit [tokeny souběžnosti](xref:core/modeling/concurrency) podrobnosti o tom, jak nakonfigurovat tokeny souběžnosti v modelu.

> [!TIP]
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) na Githubu.

_Databáze souběžnosti_ odkazuje na situace, ve kterých několik procesů nebo uživatelé přístup nebo změna stejná data v databázi ve stejnou dobu. _Řízení souběžnosti_ odkazuje na konkrétní mechanismus používaný k zajištění konzistence dat za přítomnosti souběžných změn.

EF Core implementuje _optimistického řízení souběžnosti_, což znamená, že umožní více procesů nebo uživatelé provádět změny nezávisle na sobě bez režie synchronizace nebo uzamčení. V ideálním případě tyto změny nebudou konfliktu mezi sebou a proto nebudete mít k úspěšnému. V nejhorším případě případu dvě nebo více procesů se pokusí provést konfliktní změny a pouze jeden z nich uspěli.

## <a name="how-concurrency-control-works-in-ef-core"></a>Jak funguje řízení souběžnosti v EF Core

Vlastnosti nakonfigurovat, které se používají tokeny souběžnosti implementace optimistického řízení souběžnosti: vždy, když se provádí operaci update nebo delete během `SaveChanges`, hodnota tokenem souběžnosti v databázi se porovná s původní hodnota čtení pomocí EF Core.

- Pokud se hodnoty shodují, můžete dokončit operaci.
- Pokud se hodnoty neshodují, EF Core předpokládá, že jiný uživatel provedl konfliktní operace a zruší aktuální transakce.

Situace, při které jiný uživatel provedl operace, která je v konfliktu s aktuální operace se označuje jako _konfliktů souběžnosti_.

Poskytovatelé databází zodpovídají za implementaci porovnání hodnoty tokenu souběžnosti.

U relačních databází EF Core zahrnuje kontrolu hodnoty tokenu souběžnosti v `WHERE` klauzule žádné `UPDATE` nebo `DELETE` příkazy. Po spuštění příkazů, přečte EF Core počet řádků, které byly ovlivněny.

Pokud jsou ovlivněny žádné řádky, zjištění konfliktu souběžnosti a EF Core vyvolá `DbUpdateConcurrencyException`.

Například může chceme konfigurovat `LastName` na `Person` bude tokenem souběžnosti. Pak všechny operace aktualizace na osobu, budou obsahovat kontrolu souběžnosti v `WHERE` klauzule:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Řešení konfliktů souběžnosti

Pokračování v předchozím příkladu, v případě, že jeden uživatel pokusí uložit některé změny `Person`, ale již byl změněn jiným uživatelem `LastName`, pak bude vyvolána výjimka.

V tomto okamžiku může aplikace jednoduše informovat uživatele, že aktualizace nebyla úspěšná kvůli konfliktním změnám a přesunout. Ale může být vhodné požádat uživatele, ujistěte se, že tento záznam představuje stále stejná skutečná osoba a zkuste operaci zopakovat.

Tento proces je příkladem _vyřešení konfliktu souběžnosti_.

Řešení konfliktu souběžnosti zahrnuje sloučení čekající změny z aktuální `DbContext` s hodnotami v databázi. Provedeno sloučení hodnoty se liší v závislosti na aplikaci a může přesměrováni vstup uživatele.

**Existují tři sady hodnot, které jsou k dispozici pomáhající při řešení konfliktů souběžného zpracování:**

* **Aktuální hodnoty** jsou hodnoty, které aplikace se pokusila zapisovat do databáze.

* **Původní hodnoty** jsou hodnoty, které byly původně načten z databáze, dříve, než byly provedeny žádné změny.

* **Databáze hodnoty** jsou hodnoty aktuálně uložené v databázi.

Je obecný postup pro zpracování konfliktů souběžnosti:

1. Zachytit `DbUpdateConcurrencyException` během `SaveChanges`.
2. Použití `DbUpdateConcurrencyException.Entries` připravit novou sadu změn pro ovlivněné entity.
3. Obnovte původní hodnoty tokenem souběžnosti tak, aby odrážela aktuální hodnoty v databázi.
4. Proces opakujte, dokud nedojde ke konfliktům dojít.

V následujícím příkladu `Person.FirstName` a `Person.LastName` jsou nastavené jako tokeny souběžnosti. Je `// TODO:` komentář v umístění, kde zahrnout konkrétní logiku aplikace zvolit hodnota, která má být uložen.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
