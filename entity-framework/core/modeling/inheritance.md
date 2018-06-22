---
title: Dědičnost – EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054196"
---
# <a name="inheritance"></a>Dědičnost

Dědičnost ve EF model slouží k řízení, jak je reprezentována dědičnosti ve třídách entity v databázi.

## <a name="conventions"></a>Konvence

Podle konvence je zprostředkovatel databáze k určení, jak se dědičnost nelze v databázi. V tématu [dědičnosti (relační databáze)](relational/inheritance.md) pro toto zpracování s poskytovatelem relační databáze.

EF bude pouze nastavení dědičnosti, pokud dva nebo více typů zděděné jsou explicitně součástí modelu. EF nebudou kontrolovat pro základní nebo odvozené typy, které nebyly jinak součástí modelu. Můžete zahrnout typy v modelu vystavení *DbSet<TEntity>*  pro každý typ v hierarchii dědičnosti.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Pokud nechcete, aby ke zveřejnění *DbSet<TEntity>*  pro jeden nebo více entit v hierarchii, můžete použít rozhraní Fluent API zajistit jsou zahrnuty do modelu.
A pokud nemáte spoléhají na konvence můžete určit základní typ explicitně pomocí `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Můžete použít `.HasBaseType((Type)null)` na typ entity z hierarchie odebrat.

## <a name="data-annotations"></a>Datových poznámek

Poznámky dat nelze použít ke konfiguraci dědičnosti.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API pro dědičnosti závisí na poskytovateli databáze, kterou používáte. V tématu [dědičnosti (relační databáze)](relational/inheritance.md) konfigurace můžete provést pro poskytovatele relační databáze.
