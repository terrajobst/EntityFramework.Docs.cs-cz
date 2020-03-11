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
# <a name="querying-data"></a><span data-ttu-id="dd287-102">Dotazování na data</span><span class="sxs-lookup"><span data-stu-id="dd287-102">Querying Data</span></span>

<span data-ttu-id="dd287-103">Entity Framework Core používá k dotazování dat z databáze jazykově integrovaný dotaz (LINQ).</span><span class="sxs-lookup"><span data-stu-id="dd287-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="dd287-104">LINQ umožňuje použít C# (nebo vlastní jazyk rozhraní .NET) k zápisu silně typových dotazů.</span><span class="sxs-lookup"><span data-stu-id="dd287-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries.</span></span> <span data-ttu-id="dd287-105">Používá odvozený kontext a třídy entit k odkazování databázových objektů.</span><span class="sxs-lookup"><span data-stu-id="dd287-105">It uses your derived context and entity classes to reference database objects.</span></span> <span data-ttu-id="dd287-106">EF Core předá reprezentace dotazu LINQ poskytovateli databáze.</span><span class="sxs-lookup"><span data-stu-id="dd287-106">EF Core passes a representation of the LINQ query to the database provider.</span></span> <span data-ttu-id="dd287-107">Poskytovatelé databází pak převádějí do dotazovacího jazyka specifického pro databázi (například SQL pro relační databázi).</span><span class="sxs-lookup"><span data-stu-id="dd287-107">Database providers in turn translate it to database-specific query language (for example, SQL for a relational database).</span></span>

> [!TIP]
> <span data-ttu-id="dd287-108">[Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) tohoto článku můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="dd287-108">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

<span data-ttu-id="dd287-109">Následující fragmenty kódu ukazují několik příkladů, jak dosáhnout běžných úloh pomocí Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="dd287-109">The following snippets show a few examples of how to achieve common tasks with Entity Framework Core.</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="dd287-110">Načítání všech dat</span><span class="sxs-lookup"><span data-stu-id="dd287-110">Loading all data</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a><span data-ttu-id="dd287-111">Načítá se jedna entita.</span><span class="sxs-lookup"><span data-stu-id="dd287-111">Loading a single entity</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a><span data-ttu-id="dd287-112">Filtrování</span><span class="sxs-lookup"><span data-stu-id="dd287-112">Filtering</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a><span data-ttu-id="dd287-113">Další čtení</span><span class="sxs-lookup"><span data-stu-id="dd287-113">Further readings</span></span>

- <span data-ttu-id="dd287-114">Další informace o [výrazech dotazů LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span><span class="sxs-lookup"><span data-stu-id="dd287-114">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
- <span data-ttu-id="dd287-115">Podrobnější informace o tom, jak se dotaz zpracovává v EF Core, najdete v tématu [Jak funguje dotaz](xref:core/querying/how-query-works).</span><span class="sxs-lookup"><span data-stu-id="dd287-115">For more detailed information on how a query is processed in EF Core, see [How Query Works](xref:core/querying/how-query-works).</span></span>
