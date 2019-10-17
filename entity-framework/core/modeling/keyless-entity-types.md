---
title: Typy entit bez klíčů – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 3dbc2700fc9bb277eb90885dfc2506c250ae21f1
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445942"
---
# <a name="keyless-entity-types"></a>Typy entit bez klíčů

> [!NOTE]
> Tato funkce byla přidána do EF Core 2,1 pod názvem typů dotazů. V EF Core 3,0 byl koncept přejmenován na bez klíčů entity Types.

Kromě běžných typů entit může EF Core model obsahovat _typy entit bez klíčů_, které lze použít k provádění databázových dotazů na data, která neobsahují klíčové hodnoty.

## <a name="keyless-entity-types-characteristics"></a>Vlastnosti typů entit bez klíčů

Typy entit bez klíčů podporují mnoho stejných funkcí mapování jako regulární typy entit, jako je mapování dědičnosti a vlastnosti navigace. V relačních úložištích můžou konfigurovat objekty a sloupce cílové databáze prostřednictvím metod rozhraní Fluent API nebo datových poznámek.

Liší se však od regulárních typů entit v tom, že:

- Nelze definovat klíč.
- Nejsou sledovány pro změny v _DbContext_ , a proto nejsou nikdy vloženy, aktualizovány ani smazány v databázi.
- Nejsou nikdy zjištěny konvencí.
- Podporuje pouze podmnožinu možností mapování navigace, konkrétně:
  - Nemusí nikdy fungovat jako hlavní konec relace.
  - Nemusí mít navigace ke vlastněným entitám.
  - Mohou obsahovat pouze referenční navigační vlastnosti ukazující na běžné entity.
  - Entity nemůžou obsahovat navigační vlastnosti bez klíčů typů entit.
- Je nutné nakonfigurovat s voláním metody `.HasNoKey()`.
- Může být mapován na _definiční dotaz_. Definiční dotaz je dotaz deklarovaný v modelu, který slouží jako zdroj dat pro typ entity bez klíčů.

## <a name="usage-scenarios"></a>Scénáře použití

Některé z hlavních scénářů použití pro typy entit bez klíčů jsou:

- Slouží jako návratový typ pro [nezpracované dotazy SQL](xref:core/querying/raw-sql).
- Mapování na zobrazení databáze, která neobsahují primární klíč.
- Mapování na tabulky, ve kterých není definován primární klíč.
- Mapování na dotazy definované v modelu.

## <a name="mapping-to-database-objects"></a>Mapování na databázové objekty

Mapování typu entity bez klíčů na databázový objekt se dosahuje pomocí rozhraní API `ToTable` nebo `ToView` Fluent. Z perspektivy EF Core je databázový objekt zadaný v této metodě _zobrazení_, což znamená, že je považován za zdroj dotazu jen pro čtení a nemůže být cílem operace Update, INSERT nebo DELETE. To však neznamená, že objekt databáze je skutečně vyžadován pro zobrazení databáze. Může se případně jednat o databázovou tabulku, která bude považována za jen pro čtení. U regulárních typů entit EF Core předpokládá, že databázový objekt zadaný v metodě `ToTable` lze považovat za _tabulku_, což znamená, že se dá použít jako zdroj dotazu, ale také cílený na operace aktualizace, odstranění a vložení. Ve skutečnosti můžete zadat název zobrazení databáze v `ToTable` a vše by mělo fungovat, dokud je v databázi nakonfigurováno, aby bylo možné aktualizovat.

> [!NOTE]
> `ToView` předpokládá, že objekt již v databázi existuje a nebude vytvořen migracemi.

## <a name="example"></a>Příklad

Následující příklad ukazuje, jak použít typy entit bez klíčů k dotazování zobrazení databáze.

> [!TIP]
> [Ukázku](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) tohoto článku můžete zobrazit na GitHubu.

Nejdřív definujeme jednoduchý blog a model post:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

V dalším kroku definujeme jednoduché zobrazení databáze, které nám umožní dotazovat se na počet příspěvků přidružených ke každému blogu:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

Dále definujeme třídu, která bude uchovávat výsledek z pohledu databáze:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

V dalším kroku nakonfigurujeme typ entity bez klíčů v _OnModelCreating_ pomocí rozhraní API `HasNoKey`.
Rozhraní API pro konfiguraci Fluent používáme ke konfiguraci mapování pro typ entity bez klíčů:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

Dále nakonfigurujte `DbContext` tak, aby zahrnovala `DbSet<T>`:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

Nakonec můžeme dotazovat zobrazení databáze standardním způsobem:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> Poznámka: také jsme definovali vlastnost dotazu na úrovni kontextu (Negenerickými) tak, aby fungovala jako kořen pro dotazy na tento typ.
