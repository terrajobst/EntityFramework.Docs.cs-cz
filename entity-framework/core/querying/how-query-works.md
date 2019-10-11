---
title: Jak fungují dotazy – EF Core
author: rowanmiller
ms.date: 09/26/2018
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: bc085755f39b1288f092a8b2df892c1bf82a89f1
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72186265"
---
# <a name="how-queries-work"></a><span data-ttu-id="f0c1a-102">Jak fungují dotazy</span><span class="sxs-lookup"><span data-stu-id="f0c1a-102">How Queries Work</span></span>

<span data-ttu-id="f0c1a-103">Entity Framework Core používá k dotazování dat z databáze jazykově integrovaný dotaz (LINQ).</span><span class="sxs-lookup"><span data-stu-id="f0c1a-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="f0c1a-104">LINQ umožňuje použít C# (nebo vlastní jazyk .NET) k zápisu silně typových dotazů na základě odvozeného kontextu a tříd entit.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="f0c1a-105">Životnost dotazu</span><span class="sxs-lookup"><span data-stu-id="f0c1a-105">The life of a query</span></span>

<span data-ttu-id="f0c1a-106">Následuje přehled procesu, pomocí kterého každý dotaz projde.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="f0c1a-107">Dotaz LINQ je zpracován nástrojem Entity Framework Core k sestavení reprezentace, která je připravena ke zpracování zprostředkovatelem databáze.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="f0c1a-108">Výsledkem je ukládání do mezipaměti, aby se toto zpracování nemuselo provádět pokaždé, když se dotaz spustí.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="f0c1a-109">Výsledek je předán poskytovateli databáze.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="f0c1a-110">Poskytovatel databáze určuje, které části dotazu je možné vyhodnotit v databázi.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="f0c1a-111">Tyto části dotazu jsou přeloženy do dotazovacího jazyka specifického pro databázi (například SQL pro relační databázi).</span><span class="sxs-lookup"><span data-stu-id="f0c1a-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="f0c1a-112">Jeden nebo více dotazů je odesláno do databáze a vrácená sada výsledků (výsledky jsou hodnoty z databáze, nikoli instance entity)</span><span class="sxs-lookup"><span data-stu-id="f0c1a-112">One or more queries are sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="f0c1a-113">Pro každou položku v sadě výsledků dotazu</span><span class="sxs-lookup"><span data-stu-id="f0c1a-113">For each item in the result set</span></span>
   1. <span data-ttu-id="f0c1a-114">Pokud se jedná o sledovací dotaz, EF zkontroluje, jestli data představují entitu, která už je v sledování změn pro instanci kontextu.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="f0c1a-115">Pokud ano, bude vrácena existující entita.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="f0c1a-116">Pokud ne, vytvoří se nová entita, bude nastaveno sledování změn a vrátí se nová entita.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="f0c1a-117">Pokud se jedná o dotaz bez sledování, EF zkontroluje, jestli data představují entitu, která už je v sadě výsledků dotazu pro tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-117">If this is a no-tracking query, EF checks if the data represents an entity already in the result set for this query</span></span>
      * <span data-ttu-id="f0c1a-118">Pokud ano, bude vrácena existující entita <sup>(1)</sup> .</span><span class="sxs-lookup"><span data-stu-id="f0c1a-118">If so, the existing entity is returned <sup>(1)</sup></span></span>
      * <span data-ttu-id="f0c1a-119">V takovém případě se vytvoří nová entita, která se vrátí.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-119">If not, a new entity is created and returned</span></span>

<span data-ttu-id="f0c1a-120"><sup>(1)</sup> žádné sledovací dotazy pomocí slabých odkazů udržují přehled o entitách, které už byly vráceny.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-120"><sup>(1)</sup> No tracking queries use weak references to keep track of entities that have already been returned.</span></span> <span data-ttu-id="f0c1a-121">Pokud předchozí výsledek se stejnou identitou přejde mimo rozsah a spustí se uvolňování paměti, můžete získat novou instanci entity.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-121">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="f0c1a-122">Při spuštění dotazů</span><span class="sxs-lookup"><span data-stu-id="f0c1a-122">When queries are executed</span></span>

<span data-ttu-id="f0c1a-123">Při volání operátorů LINQ stačí sestavit reprezentace dotazu v paměti.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-123">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="f0c1a-124">Dotaz je odeslán do databáze pouze v případě, že jsou výsledky spotřebovány.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-124">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="f0c1a-125">Nejběžnější operace, které mají za následek odeslání dotazu do databáze, jsou tyto:</span><span class="sxs-lookup"><span data-stu-id="f0c1a-125">The most common operations that result in the query being sent to the database are:</span></span>
* <span data-ttu-id="f0c1a-126">Iterace výsledků ve smyčce `for`</span><span class="sxs-lookup"><span data-stu-id="f0c1a-126">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="f0c1a-127">Použití operátoru, například `ToList`, `ToArray` `Single`, `Count`</span><span class="sxs-lookup"><span data-stu-id="f0c1a-127">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`</span></span>
* <span data-ttu-id="f0c1a-128">Vazba výsledků dotazu na uživatelské rozhraní</span><span class="sxs-lookup"><span data-stu-id="f0c1a-128">Databinding the results of a query to a UI</span></span>

> [!WARNING]  
> <span data-ttu-id="f0c1a-129">**Vždy ověřit vstup uživatele:** I když EF Core chrání proti útokům prostřednictvím injektáže SQL pomocí parametrů a řídicích znaků v dotazech, neověřuje vstupy.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-129">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="f0c1a-130">Příslušné ověření podle požadavků aplikace by se mělo provést před tím, než se hodnoty z nedůvěryhodných zdrojů použijí v dotazech LINQ, přiřazené k vlastnostem entity nebo předávané jiným EF Core rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-130">Appropriate validation, per the application's requirements, should be performed before values from untrusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="f0c1a-131">To zahrnuje všechny vstupy uživatele používané k dynamickému vytváření dotazů.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-131">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="f0c1a-132">I při použití LINQ, Pokud přijímáte uživatelský vstup pro vytváření výrazů, je nutné zajistit, aby bylo možné sestavit pouze zamýšlené výrazy.</span><span class="sxs-lookup"><span data-stu-id="f0c1a-132">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>
