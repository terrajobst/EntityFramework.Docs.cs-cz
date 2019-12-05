---
title: Dědičnost (relační databáze) – EF Core
description: Postup konfigurace dědičnosti typů entit v relační databázi pomocí Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 30e25aa2968ceab03404baddb46e0ae59fc3ea6b
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824754"
---
# <a name="inheritance-relational-database"></a>Dědičnost (relační databáze)

> [!NOTE]  
> Konfigurace v této části platí pro relační databáze obecně. Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).

Dědičnost v modelu EF se používá k řízení způsobu reprezentace dědičnosti tříd entit v databázi.

> [!NOTE]  
> V současné době je v EF Core implementován pouze vzor Table-per-Hierarchy (TPH). Další běžné vzorce, jako je například tabulka na typ (TPT) a tabulka-podle konkrétního typu (TPC), ještě nejsou k dispozici.

## <a name="conventions"></a>Konvence

Ve výchozím nastavení bude dědění mapováno pomocí vzoru Table-per-Hierarchy (TPH). K ukládání dat pro všechny typy v hierarchii používá typ TPH jednu tabulku. Sloupec diskriminátoru slouží k identifikaci typu, který každý řádek představuje.

EF Core bude dědění nastavení pouze v případě, že jsou do modelu explicitně zahrnuty dva nebo více děděných typů (další podrobnosti naleznete v tématu [Dědičnost](../inheritance.md) ).

Níže je příklad znázorňující jednoduchý scénář dědičnosti a data uložená v relační tabulce databáze pomocí vzoru TPH. Sloupec *diskriminátor* určuje, který typ *blogu* je uložený v každém řádku.

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![obrázek](_static/inheritance-tph-data.png)

>[!NOTE]
> Sloupce databáze se při použití mapování typu TPH automaticky v případě potřeby nepovolují s možnou hodnotou null.

## <a name="data-annotations"></a>Datové poznámky

Ke konfiguraci dědičnosti nelze použít datové poznámky.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci názvu a typu sloupce diskriminátoru a hodnot, které se používají k identifikaci každého typu v hierarchii.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a>Konfigurace vlastnosti diskriminátoru

V předchozích příkladech je diskriminátor vytvořen jako [vlastnost Shadow](xref:core/modeling/shadow-properties) v základní entitě hierarchie. Vzhledem k tomu, že se jedná o vlastnost v modelu, lze ji nakonfigurovat stejně jako jiné vlastnosti. Například pro nastavení maximální délky při použití výchozího diskriminátoru podle konvence:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/DefaultDiscriminator.cs#DiscriminatorConfiguration)]

Diskriminátor lze také namapovat na vlastnost .NET v entitě a nakonfigurovat ji. Příklad:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs#NonShadowDiscriminator)]

## <a name="shared-columns"></a>Sdílené sloupce

Pokud mají dva typy entit stejné úrovně vlastnost se stejným názvem, budou ve výchozím nastavení namapovány na dva samostatné sloupce. Pokud jsou ale kompatibilní, můžou být namapované na stejný sloupec:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs#SharedTPHColumns)]