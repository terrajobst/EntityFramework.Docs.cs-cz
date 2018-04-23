---
title: Typy dotazů - EF jádra
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 4e02f106e086d243b23a60c02838f32555be210e
ms.sourcegitcommit: 26f33758c47399ae933f22fec8e1d19fa7d2c0b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/19/2018
---
# <a name="query-types"></a>Typy dotazů
> [!NOTE]
> Tato funkce je nového v EF základní 2.1

Kromě typy entit, může obsahovat model EF základní _typy dotazů_, které lze provádět dotazy na databázi pro data, která není namapován na typy entit.

Typy dotazů mít mnoho společného s typy entit:

- Mohou také být přidány do modelu buď v `OnModelCreating`, nebo prostřednictvím vlastnost "nastavení" na odvozeným _DbContext_.
- Podporují řadu stejné mapování funkce, jako je dědičnosti mapování navigační vlastnosti (viz níže omezení) a na relační úložiště umožňuje konfiguraci cílové databázové objekty a sloupce prostřednictvím metody fluent API nebo pomocí datových poznámek.

Ale liší se od entity typy v tom, že se:

- Klíč, který se má definovat nevyžadují.
- Se nikdy sledují změny na _DbContext_ a proto se nikdy vložit, aktualizovat nebo odstranit v databázi.
- Nikdy zjistí konvence.
- Podporují pouze podmnožinu možnosti mapování navigační – konkrétně, nikdy může fungovat jako hlavní konec relace.
- Řešeny na _ModelBuilder_ pomocí `Query` metoda místo `Entity` metoda.
- Jsou namapované na _DbContext_ prostřednictvím vlastnosti typu `DbQuery<T>` místo `DbSet<T>`
- Jsou namapované na databázových objektů pomocí `ToView` metoda, místo `ToTable`.
- Může být namapovaný na _definice dotazu_ – definice dotazu je sekundární dotazu deklarované v modelu, který se chová zdroje dat pro typ dotazu.

Některé scénáře hlavní využití pro typy dotazů jsou:

- Slouží jako návratový typ pro ad hoc `FromSql()` dotazy.
- Mapování pro zobrazení databáze.
- Mapování tabulek, které nemají definovaný primární klíč.
- Mapování na dotazy, které jsou definované v modelu.

> [!TIP]
> Mapování typu dotaz na objekt databáze je dosaženo pomocí `ToView` rozhraní fluent API. Z pohledu EF jádra, je v této metodě zadaného objektu databáze _zobrazení_, což znamená, že se zpracovává jako zdroj dotazu jen pro čtení a nemůže být cílem aktualizace, vložit nebo operace odstranění. Však neznamená to, že databázový objekt je ve skutečnosti nemusí být zobrazení databáze – může být případně tabulku databáze, která budou zpracovány jako jen pro čtení. Naopak pro typy entit, EF základní předpokládá, že databázový objekt zadaný v `ToTable` metoda lze zacházet jako _tabulky_, což znamená, že lze použít jako zdroj dotazu, ale také napojeno aktualizaci, odstranění a vložení operace. Ve skutečnosti můžete zadat název zobrazení databáze v `ToTable` a všechno, co by měla fungovat správně, dokud zobrazení nakonfigurovaný tak, aby byl aktualizovat v databázi.

## <a name="example"></a>Příklad

Následující příklad ukazuje, jak chcete použít typ dotazu do databáze zobrazení dotazu.

> [!TIP]
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) na Githubu.

Nejprve definujeme jednoduchého modelu Blog a Post:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

V dalším kroku jsme definovali zobrazení jednoduché databáze, které vám umožní nám dotaz počet příspěvcích spojené s každou blogu:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

V dalším kroku jsme definovali třída pro uložení výsledek z pohledu databáze:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

V dalším kroku nakonfigurujeme typ dotazu v _OnModelCreating_ pomocí `modelBuilder.Query<T>` rozhraní API.
Můžeme použít standardní fluent konfigurace rozhraní API pro konfiguraci mapování pro typ dotazu:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

Nakonec jsme dotazu na zobrazení databáze ve standardním způsobem:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Všimněte si, že jsme definovali také vlastnost úrovni dotazu (DbQuery) tak, aby fungoval jako kořenový adresář pro dotazy pro tento typ kontextu.
