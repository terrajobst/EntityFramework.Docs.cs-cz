---
title: Indexy (relační databáze) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 7bb74d0bfa6090b597eb988a46f00494e25f233e
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813636"
---
# <a name="indexes-relational-database"></a><span data-ttu-id="536af-102">Indexy (relační databáze)</span><span class="sxs-lookup"><span data-stu-id="536af-102">Indexes (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="536af-103">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="536af-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="536af-104">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="536af-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="536af-105">Index v relační databázi se mapuje na stejný koncept jako index v jádru Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="536af-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="536af-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="536af-106">Conventions</span></span>

<span data-ttu-id="536af-107">Podle konvence jsou indexy pojmenovány `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="536af-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="536af-108">U složených indexů `<property name>` se jako seznam názvů vlastností oddělí podtržítko.</span><span class="sxs-lookup"><span data-stu-id="536af-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="536af-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="536af-109">Data Annotations</span></span>

<span data-ttu-id="536af-110">Indexy nelze konfigurovat pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="536af-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="536af-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="536af-111">Fluent API</span></span>

<span data-ttu-id="536af-112">Ke konfiguraci názvu indexu můžete použít rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="536af-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="536af-113">Můžete také zadat filtr.</span><span class="sxs-lookup"><span data-stu-id="536af-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="536af-114">Při použití poskytovatele SQL Server EF Přidá filtr IS NOT NULL pro všechny sloupce s možnou hodnotou null, které jsou součástí jedinečného indexu.</span><span class="sxs-lookup"><span data-stu-id="536af-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="536af-115">K přepsání této konvence můžete uvést `null` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="536af-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a><span data-ttu-id="536af-116">Zahrnout sloupce do indexů SQL Server</span><span class="sxs-lookup"><span data-stu-id="536af-116">Include Columns in SQL Server Indexes</span></span>

<span data-ttu-id="536af-117">Pokud jsou všechny sloupce v dotazu zahrnuté do indexu jako klíčové nebo neklíčové sloupce, můžete nakonfigurovat [indexy se zahrnutými sloupci](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) a významně tak zvýšit výkon dotazů.</span><span class="sxs-lookup"><span data-stu-id="536af-117">You can configure [indexes with included columns](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) to significantly improve query performance when all columns in the query are included in the index as key or non-key columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ForSqlServerHasIndex.cs?name=Model)]
