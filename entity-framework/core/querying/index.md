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
# <a name="querying-data"></a><span data-ttu-id="1f90d-102">Dotazování na data</span><span class="sxs-lookup"><span data-stu-id="1f90d-102">Querying Data</span></span>

<span data-ttu-id="1f90d-103">Jádro entity framework používá jazykový integrovaný dotaz (LINQ) k dotazování dat z databáze.</span><span class="sxs-lookup"><span data-stu-id="1f90d-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="1f90d-104">LINQ umožňuje používat C# (nebo váš jazyk .NET volby) psát dotazy silného typu.</span><span class="sxs-lookup"><span data-stu-id="1f90d-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries.</span></span> <span data-ttu-id="1f90d-105">Používá odvozené kontext a entity třídy pro odkaz databázové objekty.</span><span class="sxs-lookup"><span data-stu-id="1f90d-105">It uses your derived context and entity classes to reference database objects.</span></span> <span data-ttu-id="1f90d-106">EF Core předá reprezentaci dotazu LINQ poskytovateli databáze.</span><span class="sxs-lookup"><span data-stu-id="1f90d-106">EF Core passes a representation of the LINQ query to the database provider.</span></span> <span data-ttu-id="1f90d-107">Zprostředkovatelé databáze zase přeložit do dotazovacího jazyka specifické ho v databázi (například SQL pro relační databáze).</span><span class="sxs-lookup"><span data-stu-id="1f90d-107">Database providers in turn translate it to database-specific query language (for example, SQL for a relational database).</span></span>

> [!TIP]
> <span data-ttu-id="1f90d-108">Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="1f90d-108">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

<span data-ttu-id="1f90d-109">Následující úryvky ukazují několik příkladů, jak dosáhnout běžných úkolů s jádrem entity frameworku.</span><span class="sxs-lookup"><span data-stu-id="1f90d-109">The following snippets show a few examples of how to achieve common tasks with Entity Framework Core.</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="1f90d-110">Načítání všech dat</span><span class="sxs-lookup"><span data-stu-id="1f90d-110">Loading all data</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a><span data-ttu-id="1f90d-111">Načítání jedné entity</span><span class="sxs-lookup"><span data-stu-id="1f90d-111">Loading a single entity</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a><span data-ttu-id="1f90d-112">Filtrování</span><span class="sxs-lookup"><span data-stu-id="1f90d-112">Filtering</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a><span data-ttu-id="1f90d-113">Další hodnoty</span><span class="sxs-lookup"><span data-stu-id="1f90d-113">Further readings</span></span>

- <span data-ttu-id="1f90d-114">Další informace o [výrazech dotazů LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span><span class="sxs-lookup"><span data-stu-id="1f90d-114">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
- <span data-ttu-id="1f90d-115">Podrobnější informace o tom, jak je dotaz zpracován v ef core, naleznete v [tématu Jak funguje dotaz](xref:core/querying/how-query-works).</span><span class="sxs-lookup"><span data-stu-id="1f90d-115">For more detailed information on how a query is processed in EF Core, see [How Query Works](xref:core/querying/how-query-works).</span></span>
