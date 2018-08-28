---
title: Dědičnost – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: c5fa9d13dec8cfc3e1cac69e471f509cbbb9e4c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995893"
---
# <a name="inheritance"></a>Dědičnost

Dědičnost v modelu EF slouží k řízení, jak je reprezentovaná dědičnosti tříd entit v databázi.

## <a name="conventions"></a>Konvence

Podle konvence je až poskytovatel databáze k určení, jak se dědičnost reprezentovat v databázi. Zobrazit [dědičnost (relační databáze)](relational/inheritance.md) pro jak tento problém řeší poskytovatele relační databáze.

EF bude pouze nastavení dědičnosti, dvou nebo více zděděných typů je explicitně zahrnuta v modelu. EF nebudou zjišťovat základní nebo odvozené typy, které jinak nejsou zahrnuty v modelu. Může obsahovat typy v modelu zveřejněním *DbSet<TEntity>*  pro každý typ v hierarchii dědičnosti.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Pokud už nechcete vystavit *DbSet<TEntity>*  pro jeden nebo více entit v hierarchii, můžete použít rozhraní Fluent API zajistit jsou zahrnuty v modelu.
A pokud nemusíte spoléhat na vytváření názvů můžete určit základní typ explicitně pomocí `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Můžete použít `.HasBaseType((Type)null)` odebrat z hierarchie typu entity.

## <a name="data-annotations"></a>Datové poznámky

Datové poznámky nelze použít ke konfiguraci dědičnosti.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API dědičnosti závisí na poskytovateli databáze, které používáte. Zobrazit [dědičnost (relační databáze)](relational/inheritance.md) pro konfiguraci můžete provést pro poskytovatele relační databáze.
