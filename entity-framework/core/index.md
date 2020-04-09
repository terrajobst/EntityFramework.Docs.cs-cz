---
title: Přehled jádra rámce entity – ef jádro
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416846"
---
# <a name="entity-framework-core"></a><span data-ttu-id="8a1c5-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="8a1c5-102">Entity Framework Core</span></span>

<span data-ttu-id="8a1c5-103">Entity Framework (EF) Core je lehká, rozšiřitelná, [open source](https://github.com/aspnet/EntityFrameworkCore) a multiplatformní verze populární technologie přístupu k datům Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8a1c5-103">Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="8a1c5-104">EF Core může sloužit jako objektrelační mapovač (O/RM), který umožňuje vývojářům rozhraní .NET pracovat s databází pomocí objektů .NET a eliminuje potřebu většiny kódu přístupu k datům, který obvykle potřebují k zápisu.</span><span class="sxs-lookup"><span data-stu-id="8a1c5-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="8a1c5-105">EF Core podporuje mnoho databázových strojů, podrobnosti najdete v [tématu Zprostředkovatelé databáze.](providers/index.md)</span><span class="sxs-lookup"><span data-stu-id="8a1c5-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="8a1c5-106">The Model</span><span class="sxs-lookup"><span data-stu-id="8a1c5-106">The Model</span></span>

<span data-ttu-id="8a1c5-107">S EF Core, přístup k datům se provádí pomocí modelu.</span><span class="sxs-lookup"><span data-stu-id="8a1c5-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="8a1c5-108">Model se skládá z tříd entit a objekt kontextu, který představuje relaci s databází, což umožňuje dotazovat a ukládat data.</span><span class="sxs-lookup"><span data-stu-id="8a1c5-108">A model is made up of entity classes and a context object that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="8a1c5-109">Další informace najdete [v tématu Vytvoření modelu.](modeling/index.md)</span><span class="sxs-lookup"><span data-stu-id="8a1c5-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="8a1c5-110">Můžete vygenerovat model z existující databáze, ručně kód ovat model tak, aby odpovídal databázi, nebo použít [ef migrace](managing-schemas/migrations/index.md) k vytvoření databáze z modelu a pak ji vyvíjet podle změny modelu v průběhu času.</span><span class="sxs-lookup"><span data-stu-id="8a1c5-110">You can generate a model from an existing database, hand code a model to match your database, or use [EF Migrations](managing-schemas/migrations/index.md) to create a database from your model, and then evolve it as your model changes over time.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a><span data-ttu-id="8a1c5-111">Dotazování</span><span class="sxs-lookup"><span data-stu-id="8a1c5-111">Querying</span></span>

<span data-ttu-id="8a1c5-112">Instance tříd entity se načítají z databáze pomocí jazyka Integrovaný dotaz (LINQ).</span><span class="sxs-lookup"><span data-stu-id="8a1c5-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="8a1c5-113">Další informace najdete [v tématu Dotazování](querying/index.md) se na data.</span><span class="sxs-lookup"><span data-stu-id="8a1c5-113">See [Querying Data](querying/index.md) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a><span data-ttu-id="8a1c5-114">Ukládání dat</span><span class="sxs-lookup"><span data-stu-id="8a1c5-114">Saving Data</span></span>

<span data-ttu-id="8a1c5-115">Data jsou v databázi vytvářena, odstraňována a upravována pomocí instancí tříd entit.</span><span class="sxs-lookup"><span data-stu-id="8a1c5-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="8a1c5-116">Další informace najdete v [tématu Ukládání dat.](saving/index.md)</span><span class="sxs-lookup"><span data-stu-id="8a1c5-116">See [Saving Data](saving/index.md) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a><span data-ttu-id="8a1c5-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a1c5-117">Next steps</span></span>

<span data-ttu-id="8a1c5-118">Úvodní kurzy najdete v tématu [Začínáme s jádrem entity frameworku](get-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="8a1c5-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>
