---
title: "Jak dotazuje pracovní – základní EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 7fd2940d559f82016d7a8fc3fdcf3af0d5b8bc8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="how-queries-work"></a><span data-ttu-id="f6209-102">Jak fungují dotazy</span><span class="sxs-lookup"><span data-stu-id="f6209-102">How Queries Work</span></span>

<span data-ttu-id="f6209-103">Entity Framework Core jazykové integrují dotazu (LINQ) používá k dotazování na data z databáze.</span><span class="sxs-lookup"><span data-stu-id="f6209-103">Entity Framework Core uses Language Integrate Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="f6209-104">LINQ umožňuje používat C# (nebo vámi zvolený jazyk rozhraní .NET) pro zápis silného typu dotazů založených na odvozené třídy kontextu a entity.</span><span class="sxs-lookup"><span data-stu-id="f6209-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="f6209-105">Dobu životnosti dotazu</span><span class="sxs-lookup"><span data-stu-id="f6209-105">The life of a query</span></span>

<span data-ttu-id="f6209-106">Zde je podrobný přehled procesu, který prochází každý dotaz.</span><span class="sxs-lookup"><span data-stu-id="f6209-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="f6209-107">Dotaz LINQ zpracovává Entity Framework Core vytvořit reprezentaci, který je připravena k provedení zprostředkovatelem databáze</span><span class="sxs-lookup"><span data-stu-id="f6209-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="f6209-108">Výsledek se uloží do mezipaměti, aby toto zpracování není potřeba provést pokaždé, když je proveden dotaz</span><span class="sxs-lookup"><span data-stu-id="f6209-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="f6209-109">Výsledek je předán k poskytovateli databáze</span><span class="sxs-lookup"><span data-stu-id="f6209-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="f6209-110">Zprostředkovatel databáze identifikuje části dotazů, které lze vyhodnotit v databázi</span><span class="sxs-lookup"><span data-stu-id="f6209-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="f6209-111">Tyto části dotazu jsou převedeny na konkrétní dotazovacího jazyka pro databázi (např. SQL pro relační databázi)</span><span class="sxs-lookup"><span data-stu-id="f6209-111">These parts of the query are translated to database specific query language (e.g. SQL for a relational database)</span></span>
   3. <span data-ttu-id="f6209-112">Jeden nebo více dotazy se odesílají do databáze a sada výsledků vrácená (výsledky jsou hodnoty z databáze, není instancí entit)</span><span class="sxs-lookup"><span data-stu-id="f6209-112">One or more queries are sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="f6209-113">Pro každou položku v sadě výsledků dotazu</span><span class="sxs-lookup"><span data-stu-id="f6209-113">For each item in the result set</span></span>
   1. <span data-ttu-id="f6209-114">Pokud je dotaz sledování, EF kontroluje, pokud data reprezentuje entitu již v nástroji Sledování změn pro instanci kontextu</span><span class="sxs-lookup"><span data-stu-id="f6209-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="f6209-115">Pokud ano, vrátí se stávající entity</span><span class="sxs-lookup"><span data-stu-id="f6209-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="f6209-116">Pokud ne, se vytvoří nové entity, sledování změn je instalační program a vrátí novou entitu</span><span class="sxs-lookup"><span data-stu-id="f6209-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="f6209-117">Pokud je dotaz ne sledování, EF kontroluje, pokud data reprezentuje entitu již v sadě pro tento dotaz výsledků</span><span class="sxs-lookup"><span data-stu-id="f6209-117">If this is a no-tracking query, EF checks if the data represents an entity already in the result set for this query</span></span>
      * <span data-ttu-id="f6209-118">Pokud ano, je vrácen stávající entity <sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="f6209-118">If so, the existing entity is returned <sup>(1)</sup></span></span>
      * <span data-ttu-id="f6209-119">Pokud ne, se vytvoří a vrátí novou entitu</span><span class="sxs-lookup"><span data-stu-id="f6209-119">If not, a new entity is created and returned</span></span>

<span data-ttu-id="f6209-120"><sup>(1) </sup> Žádné dotazy sledování slabé odkazy použít ke sledování entit, které již byly vráceny.</span><span class="sxs-lookup"><span data-stu-id="f6209-120"><sup>(1)</sup> No tracking queries use weak references to keep track of entities that have already been returned.</span></span> <span data-ttu-id="f6209-121">Pokud předchozí výsledek se stejnou identitou ocitne mimo obor, a uvolňování paměti běží, zobrazí se nové instance entity.</span><span class="sxs-lookup"><span data-stu-id="f6209-121">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="f6209-122">Při provádění dotazů</span><span class="sxs-lookup"><span data-stu-id="f6209-122">When queries are executed</span></span>

<span data-ttu-id="f6209-123">Při volání LINQ operátory jednoduše vytváříte až reprezentaci v paměti dotazu.</span><span class="sxs-lookup"><span data-stu-id="f6209-123">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="f6209-124">Dotaz je pouze odeslal do databáze, pokud je výsledek zpracován.</span><span class="sxs-lookup"><span data-stu-id="f6209-124">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="f6209-125">Nejběžnější operace, jejichž výsledkem dotazu odesílány do databáze jsou:</span><span class="sxs-lookup"><span data-stu-id="f6209-125">The most common operations that result in the query being sent to the database are:</span></span>
* <span data-ttu-id="f6209-126">Iterace výsledky v `for` smyčky</span><span class="sxs-lookup"><span data-stu-id="f6209-126">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="f6209-127">Pomocí operátor, jako třeba `ToList`, `ToArray`, `Single`,`Count`</span><span class="sxs-lookup"><span data-stu-id="f6209-127">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`</span></span>
* <span data-ttu-id="f6209-128">Datové vazby výsledků dotazu do uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="f6209-128">Databinding the results of a query to a UI</span></span>

> [!WARNING]  
> <span data-ttu-id="f6209-129">**Vždy ověření vstupu uživatele:** při EF poskytuje ochranu před útoky vkládání SQL, neprovádí žádné obecné ověření vstupu.</span><span class="sxs-lookup"><span data-stu-id="f6209-129">**Always validate user input:** While EF does provide protection from SQL injection attacks, it does not do any general validation of input.</span></span> <span data-ttu-id="f6209-130">Proto pokud hodnoty, které jsou předávány do rozhraní API, používaná v dotazech LINQ přiřazené vlastnosti entity, atd., pocházejí z nedůvěryhodného zdroje pak příslušné ověření podle požadavků na aplikaci, je třeba provést.</span><span class="sxs-lookup"><span data-stu-id="f6209-130">Therefore if values being passed to APIs, used in LINQ queries, assigned to entity properties, etc., come from an untrusted source then appropriate validation, per your application requirements, should be performed.</span></span> <span data-ttu-id="f6209-131">To zahrnuje vstupu uživatele umožňuje dynamicky vytvářet dotazy.</span><span class="sxs-lookup"><span data-stu-id="f6209-131">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="f6209-132">I když se používá LINQ, pokud je přijímáte zadání uživatele ve výrazy, které potřebujete, abyste měli jistotu, než jenom zamýšlený výrazy se vytvářejí lze sestavit.</span><span class="sxs-lookup"><span data-stu-id="f6209-132">Even when using LINQ, if you are accepting user input to build expressions you need to make sure than only intended expressions can be constructed.</span></span>
