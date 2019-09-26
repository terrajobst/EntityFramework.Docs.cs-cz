---
title: Typy entit bez klíčů – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: e78b9f91fd2505de300ced7b5e73291b5d1ad3b4
ms.sourcegitcommit: 7bc43f21e7bdd64926314ea949aae689f1911956
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266778"
---
# <a name="keyless-entity-types"></a>Typy entit bez klíčů
> [!NOTE]
> Tato funkce byla přidána do EF Core 2,1 pod názvem typů dotazů. V EF Core 3,0 byl koncept přejmenován na bez klíčů entity Types.

Kromě běžných typů entit může EF Core model obsahovat _typy entit bez klíčů_, které lze použít k provádění databázových dotazů na data, která neobsahují klíčové hodnoty.

## <a name="keyless-entity-types-characteristics"></a>Vlastnosti typů entit bez klíčů

Typy entit bez klíčů podporují mnoho stejných funkcí mapování jako regulární typy entit, jako je mapování dědičnosti a vlastnosti navigace. Na relační úložiště můžete nakonfigurovat cílové objektů databáze a sloupce pomocí metody fluent API nebo datových poznámek.

Liší se však od regulárních typů entit v tom, že:

- Nelze definovat klíč.
- Nejsou sledovány pro změny v _DbContext_ , a proto nejsou nikdy vloženy, aktualizovány ani smazány v databázi.
- Nikdy zjištění konvencí.
- Podporuje pouze podmnožinu možností mapování navigace, konkrétně:
  - Může se nikdy fungovat jako hlavní konec relace.
  - Nemusí mít navigace ke vlastněným entitám.
  - Mohou obsahovat pouze referenční navigační vlastnosti ukazující na běžné entity.
  - Entity nemůžou obsahovat navigační vlastnosti bez klíčů typů entit.
- Je nutné nakonfigurovat s `.HasNoKey()` voláním metody.
- Může být mapován na _definiční dotaz_. Definiční dotaz je dotaz deklarovaný v modelu, který slouží jako zdroj dat pro typ entity bez klíčů.

## <a name="usage-scenarios"></a>Scénáře použití

Některé z hlavních scénářů použití pro typy entit bez klíčů jsou:

- Slouží jako návratový typ pro [nezpracované dotazy SQL](xref:core/querying/raw-sql).
- Mapování na zobrazení databáze, která neobsahují primární klíč.
- Mapování tabulek, které nemají definován primární klíč.
- Mapování pro dotazy definované v modelu.

## <a name="mapping-to-database-objects"></a>Mapování databázových objektů

Mapování typu entity bez klíčů k databázovému objektu se dosáhne pomocí `ToTable` rozhraní API Fluent nebo. `ToView` Z pohledu EF Core je určený v této metodě objekt databáze _zobrazení_, to znamená, že je považován za zdroj dotazu jen pro čtení a nemůže být cílem příkazu update, insert nebo operace odstranění. To však neznamená, že objekt databáze je skutečně vyžadován pro zobrazení databáze. Může se případně jednat o databázovou tabulku, která bude považována za jen pro čtení. U regulárních typů entit EF Core předpokládá, že databázový objekt zadaný v `ToTable` metodě může být zpracován jako _tabulka_, což znamená, že je možné jej použít jako zdroj dotazu, ale také cílený na operace Update, DELETE a INSERT. Ve skutečnosti můžete zadat název databáze zobrazení v `ToTable` a všechno, co by mělo fungovat bez problémů jako zobrazení konfigurován tak, aby umožnit aktualizaci modelové databáze.

> [!NOTE]
> `ToView`předpokládá, že objekt již v databázi existuje a nebude vytvořen migracemi.

## <a name="example"></a>Příklad

Následující příklad ukazuje, jak použít typy entit bez klíčů k dotazování zobrazení databáze.

> [!TIP]
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) na Githubu.

Nejprve definujte jsme jednoduchý model blogu a příspěvek:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

Dále nadefinujeme zobrazení jednoduché databáze, které vám umožní nám zjistit počet příspěvků, které jsou spojené s každou blogu:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

V dalším kroku budeme definovat třídu pro uchování výsledku ze zobrazení databáze:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

V dalším kroku nakonfigurujeme typ entity bez klíčů v _OnModelCreating_ pomocí `HasNoKey` rozhraní API.
Rozhraní API pro konfiguraci Fluent používáme ke konfiguraci mapování pro typ entity bez klíčů:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

Dále nakonfigurujeme `DbContext` tak, aby `DbSet<T>`zahrnovala:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

Nakonec jsme zobrazení databáze můžete dotazovat na standardním způsobem:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> Poznámka: také jsme definovali vlastnost dotazu na úrovni kontextu (Negenerickými) tak, aby fungovala jako kořen pro dotazy na tento typ.
