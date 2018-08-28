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
# <a name="indexes"></a><span data-ttu-id="cd266-102">Indexy</span><span class="sxs-lookup"><span data-stu-id="cd266-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="cd266-103">Obecně se vztahuje k relačním databázím konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="cd266-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="cd266-104">Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).</span><span class="sxs-lookup"><span data-stu-id="cd266-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="cd266-105">Index v relační databázi mapuje na stejný koncept jako index v samotném rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cd266-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="cd266-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="cd266-106">Conventions</span></span>

<span data-ttu-id="cd266-107">Podle konvence jsou pojmenovány indexy `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="cd266-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="cd266-108">Pro složené indexy `<property name>` stane podtržítka oddělený seznam názvů vlastností.</span><span class="sxs-lookup"><span data-stu-id="cd266-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="cd266-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="cd266-109">Data Annotations</span></span>

<span data-ttu-id="cd266-110">Indexy nelze konfigurovat pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="cd266-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="cd266-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="cd266-111">Fluent API</span></span>

<span data-ttu-id="cd266-112">Rozhraní Fluent API můžete použít ke konfiguraci název indexu.</span><span class="sxs-lookup"><span data-stu-id="cd266-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="cd266-113">Můžete také zadat filtr.</span><span class="sxs-lookup"><span data-stu-id="cd266-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="cd266-114">Při použití zprostředkovatele SQL Server EF přidá 'IS NOT NULL' filtr pro všechny sloupce s možnou hodnotou Null, které jsou součástí jedinečný index.</span><span class="sxs-lookup"><span data-stu-id="cd266-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="cd266-115">Chcete-li přepsat touto konvencí, můžete zadat `null` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cd266-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
