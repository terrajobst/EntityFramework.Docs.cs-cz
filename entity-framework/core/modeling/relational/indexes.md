---
title: Indexy – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993214"
---
# <a name="indexes"></a>Indexy

> [!NOTE]  
> Obecně se vztahuje k relačním databázím konfigurace v této části. Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).

Index v relační databázi mapuje na stejný koncept jako index v samotném rozhraní Entity Framework.

## <a name="conventions"></a>Konvence

Podle konvence jsou pojmenovány indexy `IX_<type name>_<property name>`. Pro složené indexy `<property name>` stane podtržítka oddělený seznam názvů vlastností.

## <a name="data-annotations"></a>Datové poznámky

Indexy nelze konfigurovat pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci název indexu.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

Můžete také zadat filtr.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

Při použití zprostředkovatele SQL Server EF přidá 'IS NOT NULL' filtr pro všechny sloupce s možnou hodnotou Null, které jsou součástí jedinečný index. Chcete-li přepsat touto konvencí, můžete zadat `null` hodnotu.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
