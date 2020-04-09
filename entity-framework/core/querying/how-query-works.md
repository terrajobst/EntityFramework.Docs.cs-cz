---
title: Jak fungují dotazy – ef core
author: ajcvickers
ms.date: 03/17/2020
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: e8a50efe31468ea8df211602636dd474550bc0ef
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136244"
---
# <a name="how-queries-work"></a><span data-ttu-id="b0ea9-102">Jak dotazy fungují</span><span class="sxs-lookup"><span data-stu-id="b0ea9-102">How Queries Work</span></span>

<span data-ttu-id="b0ea9-103">Jádro entity framework používá jazykový integrovaný dotaz (LINQ) k dotazování dat z databáze.</span><span class="sxs-lookup"><span data-stu-id="b0ea9-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="b0ea9-104">LINQ umožňuje používat C# (nebo váš jazyk .NET volby) k zápisu dotazů silného typu na základě odvozeného kontextu a tříd entity.</span><span class="sxs-lookup"><span data-stu-id="b0ea9-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="b0ea9-105">Životnost dotazu</span><span class="sxs-lookup"><span data-stu-id="b0ea9-105">The life of a query</span></span>

<span data-ttu-id="b0ea9-106">Následuje přehled na vysoké úrovni procesu, kterým každý dotaz prochází.</span><span class="sxs-lookup"><span data-stu-id="b0ea9-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="b0ea9-107">Dotaz LINQ je zpracován jádrem entity frameworku k vytvoření reprezentace, která je připravena ke zpracování poskytovatelem databáze.</span><span class="sxs-lookup"><span data-stu-id="b0ea9-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="b0ea9-108">Výsledek je uložen do mezipaměti, takže toto zpracování není nutné provádět při každém spuštění dotazu</span><span class="sxs-lookup"><span data-stu-id="b0ea9-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="b0ea9-109">Výsledek je předán poskytovateli databáze.</span><span class="sxs-lookup"><span data-stu-id="b0ea9-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="b0ea9-110">Poskytovatel databáze identifikuje, které části dotazu lze vyhodnocovat v databázi.</span><span class="sxs-lookup"><span data-stu-id="b0ea9-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="b0ea9-111">Tyto části dotazu jsou přeloženy do databázového dotazovacího jazyka (například SQL pro relační databázi).</span><span class="sxs-lookup"><span data-stu-id="b0ea9-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="b0ea9-112">Dotaz je odeslán do databáze a vrácena sada výsledků (výsledky jsou hodnoty z databáze, nikoli instance entit)</span><span class="sxs-lookup"><span data-stu-id="b0ea9-112">A query is sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="b0ea9-113">Pro každou položku v sadě výsledků</span><span class="sxs-lookup"><span data-stu-id="b0ea9-113">For each item in the result set</span></span>
   1. <span data-ttu-id="b0ea9-114">Pokud se jedná o dotaz sledování, EF zkontroluje, zda data představují entitu již v sledování změn pro instanci kontextu</span><span class="sxs-lookup"><span data-stu-id="b0ea9-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="b0ea9-115">Pokud ano, je stávající účetní jednotka vrácena</span><span class="sxs-lookup"><span data-stu-id="b0ea9-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="b0ea9-116">Pokud tomu tak není, je vytvořena nová entita, nastavení sledování změn a vrácena nová entita.</span><span class="sxs-lookup"><span data-stu-id="b0ea9-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="b0ea9-117">Pokud se jedná o dotaz bez sledování, je vždy vytvořena a vrácena nová entita.</span><span class="sxs-lookup"><span data-stu-id="b0ea9-117">If this is a no-tracking query, then a new entity is always created and returned</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="b0ea9-118">Při provádění dotazů</span><span class="sxs-lookup"><span data-stu-id="b0ea9-118">When queries are executed</span></span>

<span data-ttu-id="b0ea9-119">Při volání linq operátory, jsou jednoduše vytváření reprezentace v paměti dotazu.</span><span class="sxs-lookup"><span data-stu-id="b0ea9-119">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="b0ea9-120">Dotaz je odeslán do databáze pouze v případě, že jsou spotřebovány výsledky.</span><span class="sxs-lookup"><span data-stu-id="b0ea9-120">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="b0ea9-121">Nejběžnější operace, které vedou k dotazu odesílaných do databáze jsou:</span><span class="sxs-lookup"><span data-stu-id="b0ea9-121">The most common operations that result in the query being sent to the database are:</span></span>

* <span data-ttu-id="b0ea9-122">Iterace výsledků ve `for` smyčce</span><span class="sxs-lookup"><span data-stu-id="b0ea9-122">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="b0ea9-123">Použití operátoru, `ToList` `ToArray`například `Count` , , `Single`nebo ekvivalentního asynchronního přetížení</span><span class="sxs-lookup"><span data-stu-id="b0ea9-123">Using an operator such as `ToList`, `ToArray`, `Single`, `Count` or the equivalent async overloads</span></span>

> [!WARNING]  
> <span data-ttu-id="b0ea9-124">**Vždy ověřte vstup uživatele:** Zatímco EF Core chrání před útoky injektáže SQL pomocí parametrů a escaping literály v dotazech, neověřuje vstupy.</span><span class="sxs-lookup"><span data-stu-id="b0ea9-124">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="b0ea9-125">Vhodné ověření podle požadavků aplikace by mělo být provedeno před použitím hodnot z nedůvěryhodných zdrojů v dotazech LINQ, přiřazených k vlastnostem entity nebo předáno jiným klíčovým rozhraním API EF.</span><span class="sxs-lookup"><span data-stu-id="b0ea9-125">Appropriate validation, per the application's requirements, should be performed before values from un-trusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="b0ea9-126">To zahrnuje všechny vstupy uživatele slouží k dynamicky vytvářet dotazy.</span><span class="sxs-lookup"><span data-stu-id="b0ea9-126">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="b0ea9-127">I při použití LINQ, pokud přijímáte vstup uživatele k vytváření výrazů, musíte se ujistit, že lze sestavit pouze zamýšlené výrazy.</span><span class="sxs-lookup"><span data-stu-id="b0ea9-127">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>
