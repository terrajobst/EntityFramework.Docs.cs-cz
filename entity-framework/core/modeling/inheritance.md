---
title: Dědičnost – EF Core
description: Jak nakonfigurovat dědičnost typu entity pomocí Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 507854e3acc0347adee612e516b3e2e0b10f55cf
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417293"
---
# <a name="inheritance"></a>Dědičnost

EF může mapovat hierarchii typů .NET na databázi. To vám umožňuje psát entity .NET v kódu jako obvykle pomocí základních a odvozených typů a mít EF bez problémů vytvořit příslušné schéma databáze, vydávat dotazy atd. Skutečné podrobnosti o mapování hierarchie typů jsou závislé na poskytovateli. Tato stránka popisuje podporu dědičnosti v kontextu relační databáze.

V tuto chvíli EF Core podporuje jenom vzor typu tabulka na hierarchii (TPH). Typ TPH používá jedinou tabulku k uložení dat pro všechny typy v hierarchii a sloupec diskriminátoru slouží k identifikaci, který typ každý řádek představuje.

> [!NOTE]
> TPT se zatím nepodporují EF Core pomocí tabulky pro typ (TPC) a tabulky podle konkrétního typu (), které podporuje EF6. TPT je hlavní funkce naplánovaná pro EF Core 5,0.

## <a name="entity-type-hierarchy-mapping"></a>Mapování hierarchie typu entity

Podle konvence, EF nastaví dědění pouze v případě, že jsou do modelu explicitně zahrnuty dva nebo více děděných typů. EF nebude automaticky vyhledávat základní nebo odvozené typy, které nejsou jinak zahrnuty v modelu.

Do modelu můžete zahrnout typy tím, že vystavíte Negenerickými pro každý typ v hierarchii dědičnosti:

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?name=InheritanceDbSets&highlight=3-4)]

Tento model je namapován na následující schéma databáze (Poznamenejte si implicitně vytvořený sloupec *diskriminátorů* , který určuje, který typ *blogu* je uložený v jednotlivých řádcích):

![image](_static/inheritance-tph-data.png)

>[!NOTE]
> Sloupce databáze se při použití mapování typu TPH automaticky v případě potřeby nepovolují s možnou hodnotou null. Například sloupec *RssUrl* může mít hodnotu null, protože běžné instance *blogu* tuto vlastnost nemají.

Pokud nechcete zveřejnit Negenerickými pro jednu nebo více entit v hierarchii, můžete také použít rozhraní Fluent API, abyste zajistili, že jsou zahrnuté v modelu.

> [!TIP]
> Pokud nespoléháte na konvence, můžete základní typ explicitně zadat pomocí `HasBaseType`. K odebrání typu entity z hierarchie můžete použít taky `.HasBaseType((Type)null)`.

## <a name="discriminator-configuration"></a>Konfigurace diskriminátorů

Můžete nakonfigurovat název a typ sloupce diskriminátoru a hodnoty, které se použijí k identifikaci každého typu v hierarchii:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorConfiguration.cs?name=DiscriminatorConfiguration&highlight=4-6)]

V předchozích příkladech byl EF přidán diskriminátor implicitně jako [vlastnost Shadow](xref:core/modeling/shadow-properties) v základní entitě hierarchie. Tato vlastnost se dá nakonfigurovat jako jakákoli jiná:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorPropertyConfiguration.cs?name=DiscriminatorPropertyConfiguration&highlight=4-5)]

A konečně může být diskriminátor také mapován na regulární vlastnost .NET v entitě:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs?name=NonShadowDiscriminator&highlight=4)]

## <a name="shared-columns"></a>Sdílené sloupce

Ve výchozím nastavení, když dva typy entit na stejné úrovni v hierarchii mají vlastnost se stejným názvem, budou namapovány na dva samostatné sloupce. Pokud je však jejich typ identický, lze je namapovat na stejný sloupec databáze:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs?name=SharedTPHColumns&highlight=9,13)]
