---
title: Typy entit bez klíčů – EF Core
description: Jak nakonfigurovat typy entit bez klíčů pomocí Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 9/13/2019
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 520c9ed93240c05deee36fa527a3757490fd7082
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417314"
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
- Je nutné nakonfigurovat `.HasNoKey()` volání metody.
- Může být mapován na _definiční dotaz_. Definiční dotaz je dotaz deklarovaný v modelu, který slouží jako zdroj dat pro typ entity bez klíčů.

## <a name="usage-scenarios"></a>Scénáře použití

Některé z hlavních scénářů použití pro typy entit bez klíčů jsou:

- Slouží jako návratový typ pro [nezpracované dotazy SQL](xref:core/querying/raw-sql).
- Mapování na zobrazení databáze, která neobsahují primární klíč.
- Mapování tabulek, které nemají definován primární klíč.
- Mapování pro dotazy definované v modelu.

## <a name="mapping-to-database-objects"></a>Mapování databázových objektů

Mapování typu entity bez klíčů k databázovému objektu se dosahuje pomocí rozhraní API pro `ToTable` nebo `ToView` Fluent. Z perspektivy EF Core je databázový objekt zadaný v této metodě _zobrazení_, což znamená, že je považován za zdroj dotazu jen pro čtení a nemůže být cílem operace Update, INSERT nebo DELETE. To však neznamená, že objekt databáze je skutečně vyžadován pro zobrazení databáze. Může se případně jednat o databázovou tabulku, která bude považována za jen pro čtení. U regulárních typů entit EF Core předpokládá, že databázový objekt zadaný v metodě `ToTable` lze považovat za _tabulku_, což znamená, že se dá použít jako zdroj dotazu, ale také cílený na operace aktualizace, odstranění a vložení. Ve skutečnosti můžete zadat název zobrazení databáze v `ToTable` a vše by mělo fungovat, pokud je v databázi nakonfigurované tak, aby bylo možné aktualizovat.

> [!NOTE]
> `ToView` předpokládá, že objekt již v databázi existuje a nebude vytvořen migracemi.

## <a name="example"></a>Příklad

Následující příklad ukazuje, jak použít typy entit bez klíčů k dotazování zobrazení databáze.

> [!TIP]
> [Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) tohoto článku můžete zobrazit na GitHubu.

Nejprve definujte jsme jednoduchý model blogu a příspěvek:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

Dále nadefinujeme zobrazení jednoduché databáze, které vám umožní nám zjistit počet příspěvků, které jsou spojené s každou blogu:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

V dalším kroku budeme definovat třídu pro uchování výsledku ze zobrazení databáze:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

V dalším kroku nakonfigurujeme typ entity bez klíčů v _OnModelCreating_ pomocí rozhraní API pro `HasNoKey`.
Rozhraní API pro konfiguraci Fluent používáme ke konfiguraci mapování pro typ entity bez klíčů:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

Dále nakonfigurujeme `DbContext`, aby zahrnovala `DbSet<T>`:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

Nakonec jsme zobrazení databáze můžete dotazovat na standardním způsobem:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> Poznámka: také jsme definovali vlastnost dotazu na úrovni kontextu (Negenerickými) tak, aby fungovala jako kořen pro dotazy na tento typ.
