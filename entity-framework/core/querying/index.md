---
title: Dotazování na data – EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 0e1e50d1a3f647d65301552d0a447f9fcae81438
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417687"
---
# <a name="querying-data"></a>Dotazování na data

Entity Framework Core používá k dotazování dat z databáze jazykově integrovaný dotaz (LINQ). LINQ umožňuje použít C# (nebo vlastní jazyk rozhraní .NET) k zápisu silně typových dotazů. Používá odvozený kontext a třídy entit k odkazování databázových objektů. EF Core předá reprezentace dotazu LINQ poskytovateli databáze. Poskytovatelé databází pak převádějí do dotazovacího jazyka specifického pro databázi (například SQL pro relační databázi).

> [!TIP]
> [Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) tohoto článku můžete zobrazit na GitHubu.

Následující fragmenty kódu ukazují několik příkladů, jak dosáhnout běžných úloh pomocí Entity Framework Core.

## <a name="loading-all-data"></a>Načítání všech dat

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a>Načítá se jedna entita.

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a>Filtrování

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a>Další čtení

- Další informace o [výrazech dotazů LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)
- Podrobnější informace o tom, jak se dotaz zpracovává v EF Core, najdete v tématu [Jak funguje dotaz](xref:core/querying/how-query-works).
