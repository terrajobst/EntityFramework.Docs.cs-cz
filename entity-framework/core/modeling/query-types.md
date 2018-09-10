---
title: Typy dotazů – EF Core
author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/query-types
ms.openlocfilehash: 54d960e2e2236e2d4185dedc48f51035f5c10e93
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250722"
---
# <a name="query-types"></a>Typy dotazů
> [!NOTE]
> Tato funkce je nového v EF Core 2.1

Kromě typů entit může obsahovat modelu EF Core _typy dotazů_, které je možné provádět databáze dotazy na data, která není namapována na typy entit.

## <a name="compare-query-types-to-entity-types"></a>Porovnání typů dotazu pro typy entit

Typy dotazů jsou jako typy entit v, které jsou:

- Může být buď přidá do modelu v `OnModelCreating` nebo přes vlastnost "set" na odvozený _DbContext_.
- Podporují mnoho stejných mapování funkcí, jako je dědičnost mapování a navigační vlastnosti. Na relační úložiště můžete nakonfigurovat cílové objektů databáze a sloupce pomocí metody fluent API nebo datových poznámek.

Ale se liší od entitu napíše, která jsou:

- Klíč je definovat nevyžadují.
- Nikdy sledovány změny na _DbContext_ a proto se nikdy přidají, aktualizovat nebo odstranit v databázi.
- Nikdy zjištění konvencí.
- Podporují pouze podmnožinu možností navigace map – konkrétně:
  - Může se nikdy fungovat jako hlavní konec relace.
  - Může obsahovat pouze vlastnosti navigace odkazu přejdete na položku entity.
  - Entity nemůže obsahovat navigačních vlastností pro typy dotazů.
- Se podrobněji probírají v _ModelBuilder_ pomocí `Query` metoda místo `Entity` metody.
- Jsou mapovány na _DbContext_ prostřednictvím vlastnosti typu `DbQuery<T>` spíše než `DbSet<T>`
- Jsou mapovány na databázových objektů pomocí `ToView` metody spíše než `ToTable`.
- Může být namapovány na _definování dotazu_ – definování dotazu je sekundární dotaz deklarované v modelu, který funguje zdroj dat pro typ dotazu.

## <a name="usage-scenarios"></a>Scénáře použití

Zde jsou některé scénáře hlavní použití pro typy dotazů:

- Slouží jako návratový typ pro ad hoc `FromSql()` dotazy.
- Mapování na zobrazení databáze.
- Mapování tabulek, které nemají definován primární klíč.
- Mapování pro dotazy definované v modelu.

## <a name="mapping-to-database-objects"></a>Mapování databázových objektů

Mapování typu dotazu k databázovému objektu je dosaženo pomocí `ToView` rozhraní fluent API. Z pohledu EF Core je určený v této metodě objekt databáze _zobrazení_, to znamená, že je považován za zdroj dotazu jen pro čtení a nemůže být cílem příkazu update, insert nebo operace odstranění. Nicméně to neznamená, že databázový objekt je ve skutečnosti musí být zobrazení databáze - může být případně tabulku databáze, která se zpracuje jako jen pro čtení. Naopak pro typy entit, EF Core předpokládá, že databázový objekt zadaný v `ToTable` metody lze zacházet jako _tabulky_, což znamená, že může sloužit jako zdroj dotazu, ale také cílí aktualizace, odstranění a vložení operace. Ve skutečnosti můžete zadat název databáze zobrazení v `ToTable` a všechno, co by mělo fungovat bez problémů jako zobrazení konfigurován tak, aby umožnit aktualizaci modelové databáze.

## <a name="example"></a>Příklad

Následující příklad ukazuje, jak používat typ dotazu do databáze zobrazení dotazu.

> [!TIP]
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryTypes) na Githubu.

Nejprve definujte jsme jednoduchý model blogu a příspěvek:

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Entities)]

Dále nadefinujeme zobrazení jednoduché databáze, které vám umožní nám zjistit počet příspěvků, které jsou spojené s každou blogu:

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#View)]

V dalším kroku budeme definovat třídu pro uchování výsledku ze zobrazení databáze:

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#QueryType)]

V dalším kroku budeme nakonfigurujte typ dotazu v _OnModelCreating_ pomocí `modelBuilder.Query<T>` rozhraní API.
Můžeme použít standardní konfigurace fluent API pro konfiguraci mapování pro typ dotazu:

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Configuration)]

Nakonec jsme zobrazení databáze můžete dotazovat na standardním způsobem:

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Poznámka: také jsme definovali vlastnost úrovně dotazu (DbQuery) tak, aby fungoval jako kořenový adresář pro dotazy na tento typ kontextu.
