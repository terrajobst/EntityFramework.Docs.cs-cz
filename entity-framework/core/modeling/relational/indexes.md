---
title: Indexy – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 7dcf27dedbde45302a462a4c41a811b9868e40bb
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197013"
---
# <a name="indexes"></a><span data-ttu-id="522ce-102">Indexy</span><span class="sxs-lookup"><span data-stu-id="522ce-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="522ce-103">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="522ce-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="522ce-104">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="522ce-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="522ce-105">Index v relační databázi se mapuje na stejný koncept jako index v jádru Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="522ce-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="522ce-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="522ce-106">Conventions</span></span>

<span data-ttu-id="522ce-107">Podle konvence jsou indexy pojmenovány `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="522ce-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="522ce-108">U složených indexů `<property name>` se jako seznam názvů vlastností oddělí podtržítko.</span><span class="sxs-lookup"><span data-stu-id="522ce-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="522ce-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="522ce-109">Data Annotations</span></span>

<span data-ttu-id="522ce-110">Indexy nelze konfigurovat pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="522ce-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="522ce-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="522ce-111">Fluent API</span></span>

<span data-ttu-id="522ce-112">Ke konfiguraci názvu indexu můžete použít rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="522ce-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="522ce-113">Můžete také zadat filtr.</span><span class="sxs-lookup"><span data-stu-id="522ce-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="522ce-114">Při použití poskytovatele SQL Server EF Přidá filtr IS NOT NULL pro všechny sloupce s možnou hodnotou null, které jsou součástí jedinečného indexu.</span><span class="sxs-lookup"><span data-stu-id="522ce-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="522ce-115">K přepsání této konvence můžete uvést `null` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="522ce-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a><span data-ttu-id="522ce-116">Zahrnout sloupce do indexů SQL Server</span><span class="sxs-lookup"><span data-stu-id="522ce-116">Include Columns in SQL Server Indexes</span></span>

<span data-ttu-id="522ce-117">Pokud jsou všechny sloupce v dotazu zahrnuté do indexu jako klíčové nebo neklíčové sloupce, můžete nakonfigurovat [indexy se zahrnutými sloupci](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) a významně tak zvýšit výkon dotazů.</span><span class="sxs-lookup"><span data-stu-id="522ce-117">You can configure [indexes with included columns](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) to significantly improve query performance when all columns in the query are included in the index as key or non-key columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ForSqlServerHasIndex.cs?name=Model)]
