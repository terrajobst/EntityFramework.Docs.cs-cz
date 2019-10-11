---
title: Dotazování na data – EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 009235c3673a414e06d1a64f9877b60e7cde97b0
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181921"
---
# <a name="querying-data"></a>Dotazování na data

Entity Framework Core používá k dotazování dat z databáze jazykově integrovaný dotaz (LINQ). LINQ umožňuje použít C# (nebo vlastní jazyk rozhraní .NET) k zápisu silně typových dotazů. Používá odvozený kontext a třídy entit k odkazování databázových objektů. EF Core předá reprezentace dotazu LINQ poskytovateli databáze. Poskytovatelé databází pak převádějí do dotazovacího jazyka specifického pro databázi (například SQL pro relační databázi).

> [!TIP]
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.

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
