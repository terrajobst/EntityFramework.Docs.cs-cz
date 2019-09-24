---
title: Dědičnost – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: 1f20c455176b5922364584f8c7688c15a4c3f0f9
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197283"
---
# <a name="inheritance"></a>Dědičnost

Dědičnost v modelu EF se používá k řízení způsobu reprezentace dědičnosti tříd entit v databázi.

## <a name="conventions"></a>Konvence

Podle konvence je až od poskytovatele databáze k určení, jak bude dědění v databázi reprezentované. Podívejte se na téma [Dědičnost (relační databáze)](relational/inheritance.md) , ve kterém se tato funkce zpracovává u poskytovatele relační databáze.

EF nastaví dědění pouze v případě, že jsou do modelu explicitně zahrnuty dva nebo více děděných typů. EF nebude hledat základní nebo odvozené typy, které nebyly jinak zahrnuty v modelu. Do modelu můžete zahrnout typy tím, že vystavíte *negenerickými<TEntity>*  pro každý typ v hierarchii dědičnosti.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Pokud nechcete zveřejnit *negenerickými<TEntity>*  pro jednu nebo více entit v hierarchii, můžete použít rozhraní Fluent API, abyste zajistili, že jsou zahrnuté v modelu.
A pokud nespoléháte na konvence, můžete základní typ zadat explicitně pomocí `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Můžete použít `.HasBaseType((Type)null)` k odebrání typu entity z hierarchie.

## <a name="data-annotations"></a>Datové poznámky

Ke konfiguraci dědičnosti nelze použít datové poznámky.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní API Fluent pro dědění závisí na použitém poskytovateli databáze. Viz téma [Dědičnost (relační databáze)](relational/inheritance.md) pro konfiguraci, kterou můžete provést pro poskytovatele relační databáze.
