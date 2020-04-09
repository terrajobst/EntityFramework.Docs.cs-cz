---
title: Dotazování dat – jádro EF
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 0e1e50d1a3f647d65301552d0a447f9fcae81438
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417687"
---
# <a name="querying-data"></a>Dotazování na data

Jádro entity framework používá jazykový integrovaný dotaz (LINQ) k dotazování dat z databáze. LINQ umožňuje používat C# (nebo váš jazyk .NET volby) psát dotazy silného typu. Používá odvozené kontext a entity třídy pro odkaz databázové objekty. EF Core předá reprezentaci dotazu LINQ poskytovateli databáze. Zprostředkovatelé databáze zase přeložit do dotazovacího jazyka specifické ho v databázi (například SQL pro relační databáze).

> [!TIP]
> Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) můžete zobrazit na GitHubu.

Následující úryvky ukazují několik příkladů, jak dosáhnout běžných úkolů s jádrem entity frameworku.

## <a name="loading-all-data"></a>Načítání všech dat

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a>Načítání jedné entity

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a>Filtrování

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a>Další hodnoty

- Další informace o [výrazech dotazů LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)
- Podrobnější informace o tom, jak je dotaz zpracován v ef core, naleznete v [tématu Jak funguje dotaz](xref:core/querying/how-query-works).
