---
title: "Typy dotazů - EF jádra"
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: d03c4b1d5635530e63b93e051cb69583718deb4e
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="query-types"></a>Typy dotazů
> [!NOTE]
> Tato funkce je nového v EF základní 2.1

Typy dotazů jsou typy výsledků dotazu jen pro čtení, které mohou být přidány do model EF jádra. Typy dotazů povolení dotazů ad-hoc (jako anonymní typy), ale jsou flexibilnější, protože mají zadané konfigurace mapování.

Jsou koncepčně podobá typy entit v tom, že:

- Jsou typy objektů POCO C#, které jsou přidány do modelu, buď v ```OnModelCreating``` pomocí ```ModelBuilder.Query``` metoda, nebo přes vlastnost DbContext "set" (pro dotaz typy tato vlastnost je zadán jako ```DbQuery<T>``` místo, ```DbSet<T>```).
- Podporují většinu stejné mapování funkcí jako typy pravidelných entit. Například mapování dědění, navigací (viz níže limitiations) a na relační úložiště umožňuje konfiguraci cílové databázi objektů schématu prostřednictvím ```ToTable```, ```HasColumn``` metody rozhraní fluent api (nebo pomocí datových poznámek).

Typy dotazů se liší od entity typy v tom, že se:

- Klíč, který se má definovat nevyžadují.
- Nikdy jsou sledovány objektem modul sledování změny.
- Nikdy zjistí konvence.
- Podporují pouze podmnožinu možnosti mapování navigační – konkrétně, nikdy může fungovat jako hlavní konec relace.
- Může být namapovaný na _definice dotazu_ -A definice dotazu je sekundární dotaz, který funguje zdroj dat pro typ dotazu.

Některé scénáře hlavní využití pro typy dotazů jsou:

- Mapování pro zobrazení databáze.
- Mapování tabulek, které nemají definovaný primární klíč.
- Slouží jako návratový typ pro ad hoc ```FromSql()``` dotazy.
- Mapování na dotazy, které jsou definované v modelu.

> [!TIP]
> Mapování typu dotaz na zobrazení databáze je dosaženo pomocí ```ToTable``` rozhraní fluent API.

## <a name="example"></a>Příklad

Následující příklad ukazuje, jak chcete použít typ dotazu do databáze zobrazení dotazu.

> [!TIP]
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) na Githubu.

Nejprve definujeme jednoduchého modelu Blog a Post:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

V dalším kroku jsme definovali zobrazení jednoduché databáze, které vám umožní nám dotaz počet příspěvcích spojené s každou blogu:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

V dalším kroku nakonfigurujeme typ dotazu v _OnModelCreating_ pomocí ```modelBuilder.Query<T>``` rozhraní API.
Můžeme použít standardní fluent konfigurace rozhraní API pro konfiguraci mapování pro typ dotazu:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

Nakonec jsme dotazu na zobrazení databáze ve standardním způsobem:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Všimněte si, že jsme definovali také vlastnost úrovni dotazu (DbQuery) tak, aby fungoval jako kořenový adresář pro dotazy pro tento typ kontextu.
