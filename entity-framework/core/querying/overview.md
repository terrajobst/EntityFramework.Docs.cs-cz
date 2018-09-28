---
title: Jak dotazuje Work – EF Core
author: rowanmiller
ms.date: 09/26/2018
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/overview
ms.openlocfilehash: 23d26f9c0ac17fc0df744f5339946947ea366911
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415728"
---
# <a name="how-queries-work"></a><span data-ttu-id="ded34-102">Jak fungují dotazy</span><span class="sxs-lookup"><span data-stu-id="ded34-102">How Queries Work</span></span>

<span data-ttu-id="ded34-103">Entity Framework Core Language Integrated Query (LINQ) používá k dotazování na data z databáze.</span><span class="sxs-lookup"><span data-stu-id="ded34-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="ded34-104">LINQ umožňuje používat C# (nebo svůj jazyk .NET) pro zápis silného typu dotazů založených na odvozené třídy kontextu a entity.</span><span class="sxs-lookup"><span data-stu-id="ded34-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="ded34-105">Běžný dotaz</span><span class="sxs-lookup"><span data-stu-id="ded34-105">The life of a query</span></span>

<span data-ttu-id="ded34-106">Tady je obecný přehled procesu, který prochází každý dotaz.</span><span class="sxs-lookup"><span data-stu-id="ded34-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="ded34-107">Entity Framework Core k sestavení, která jsou připravená pro zpracování poskytovatele databáze reprezentaci zpracovává dotazu LINQ</span><span class="sxs-lookup"><span data-stu-id="ded34-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="ded34-108">Výsledek je uložen do mezipaměti tak, aby toto zpracování není nutné provést při každém spuštění dotazu</span><span class="sxs-lookup"><span data-stu-id="ded34-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="ded34-109">Výsledek je předán k poskytovateli databáze</span><span class="sxs-lookup"><span data-stu-id="ded34-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="ded34-110">Poskytovatel databáze identifikuje části dotazů, které mohou být vyhodnoceny v databázi</span><span class="sxs-lookup"><span data-stu-id="ded34-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="ded34-111">Tyto části dotazu jsou přeloženy do databáze konkrétní dotazovacího jazyka (například SQL pro relační databáze)</span><span class="sxs-lookup"><span data-stu-id="ded34-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="ded34-112">Jeden nebo více dotazy se odesílají do databáze a sada výsledků vrácená (výsledky jsou hodnoty z databáze není instancí entit)</span><span class="sxs-lookup"><span data-stu-id="ded34-112">One or more queries are sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="ded34-113">Pro každou položku v sadě výsledků</span><span class="sxs-lookup"><span data-stu-id="ded34-113">For each item in the result set</span></span>
   1. <span data-ttu-id="ded34-114">Pokud je to dotazu sledování, EF kontroluje, pokud data představuje entitu již v nástroji Sledování změn pro instance kontextu</span><span class="sxs-lookup"><span data-stu-id="ded34-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="ded34-115">Pokud ano, vrátí se existující entity</span><span class="sxs-lookup"><span data-stu-id="ded34-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="ded34-116">V opačném případě se vytvoří nové entity, sledování změn je nastavená a vrátí novou entitu</span><span class="sxs-lookup"><span data-stu-id="ded34-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="ded34-117">Pokud je to dotazu bez sledování, EF kontroluje, pokud data představuje entitu již v sadě pro tento dotaz výsledků</span><span class="sxs-lookup"><span data-stu-id="ded34-117">If this is a no-tracking query, EF checks if the data represents an entity already in the result set for this query</span></span>
      * <span data-ttu-id="ded34-118">Pokud ano, je vrácena existující entity <sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="ded34-118">If so, the existing entity is returned <sup>(1)</sup></span></span>
      * <span data-ttu-id="ded34-119">V opačném případě se vytvoří a vrátí novou entitu</span><span class="sxs-lookup"><span data-stu-id="ded34-119">If not, a new entity is created and returned</span></span>

<span data-ttu-id="ded34-120"><sup>(1) </sup> Žádné dotazy sledování můžete sledovat, entity, které již byly vráceny slabé odkazy.</span><span class="sxs-lookup"><span data-stu-id="ded34-120"><sup>(1)</sup> No tracking queries use weak references to keep track of entities that have already been returned.</span></span> <span data-ttu-id="ded34-121">Pokud předchozí výsledek se stejnou identitou dostane mimo rozsah a uvolňování paměti běží, může se zobrazit nová instance entity.</span><span class="sxs-lookup"><span data-stu-id="ded34-121">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="ded34-122">Při provádění dotazů</span><span class="sxs-lookup"><span data-stu-id="ded34-122">When queries are executed</span></span>

<span data-ttu-id="ded34-123">Při volání operátory LINQ, jednoduše vytváříte nahoru v paměti reprezentace dotazu.</span><span class="sxs-lookup"><span data-stu-id="ded34-123">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="ded34-124">Dotaz je odeslán do databáze, pouze při výsledky se spotřebuje.</span><span class="sxs-lookup"><span data-stu-id="ded34-124">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="ded34-125">Nejběžnější operace, jejichž výsledkem dotazů odesílaných do databáze jsou:</span><span class="sxs-lookup"><span data-stu-id="ded34-125">The most common operations that result in the query being sent to the database are:</span></span>
* <span data-ttu-id="ded34-126">Výsledky v iteraci `for` smyčky</span><span class="sxs-lookup"><span data-stu-id="ded34-126">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="ded34-127">Například použití operátoru `ToList`, `ToArray`, `Single`, `Count`</span><span class="sxs-lookup"><span data-stu-id="ded34-127">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`</span></span>
* <span data-ttu-id="ded34-128">Vázání dat výsledků dotazu do uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="ded34-128">Databinding the results of a query to a UI</span></span>

> [!WARNING]  
> <span data-ttu-id="ded34-129">**Vždy ověření vstupu uživatele:** při EF Core chrání před útoky prostřednictvím injektáže SQL s použitím parametrů a uvození literály v dotazech, neověřuje při vstupy.</span><span class="sxs-lookup"><span data-stu-id="ded34-129">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="ded34-130">Odpovídající ověření podle požadavků vaší aplikace, je třeba provést před použít v dotazech LINQ, přiřazená vlastností entity nebo předán pro jiná rozhraní API EF Core hodnoty z nedůvěryhodných zdrojů.</span><span class="sxs-lookup"><span data-stu-id="ded34-130">Appropriate validation, per the application's requirements, should be performed before values from untrusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="ded34-131">To zahrnuje vstupu uživatele použít k vytvoření dotazů.</span><span class="sxs-lookup"><span data-stu-id="ded34-131">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="ded34-132">I když se používá LINQ, pokud jsou přijímat uživatelský vstup k sestavení výrazy, budete muset Ujistěte se, že lze sestavit pouze odpovídající výrazy.</span><span class="sxs-lookup"><span data-stu-id="ded34-132">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>
