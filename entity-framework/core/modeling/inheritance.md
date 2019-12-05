---
title: Dědičnost – EF Core
description: Jak nakonfigurovat dědičnost typu entity pomocí Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 4d43a432174c92ab7f3f9d78a234aefb0a4a17e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824673"
---
# <a name="inheritance"></a>Dědičnost

Dědičnost v modelu EF se používá k řízení způsobu reprezentace dědičnosti tříd entit v databázi.

## <a name="conventions"></a>Konvence

Ve výchozím nastavení je to poskytovatel databáze, aby bylo možné určit, jak bude dědění v databázi reprezentované. Podívejte se na téma [Dědičnost (relační databáze)](relational/inheritance.md) , ve kterém se tato funkce zpracovává u poskytovatele relační databáze.

EF nastaví dědění pouze v případě, že jsou do modelu explicitně zahrnuty dva nebo více děděných typů. EF nebude hledat základní nebo odvozené typy, které nebyly jinak zahrnuty v modelu. Do modelu můžete zahrnout typy tím, že vystavíte *negenerickými\<TEntity >* pro každý typ v hierarchii dědičnosti.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Pokud nechcete zveřejnit *negenerickými\<TEntity >* pro jednu nebo více entit v hierarchii, můžete použít rozhraní Fluent API, abyste zajistili, že jsou zahrnuté v modelu.
A pokud nespoléháte na konvence, můžete základní typ explicitně zadat pomocí `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Pomocí `.HasBaseType((Type)null)` můžete z hierarchie odebrat typ entity.

## <a name="data-annotations"></a>Datové poznámky

Ke konfiguraci dědičnosti nelze použít datové poznámky.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní API Fluent pro dědění závisí na použitém poskytovateli databáze. Viz téma [Dědičnost (relační databáze)](relational/inheritance.md) pro konfiguraci, kterou můžete provést pro poskytovatele relační databáze.
