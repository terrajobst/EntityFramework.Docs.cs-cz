---
title: Sledování vs. Žádné dotazy sledování – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: d93be5c2b727d8fbaddd103f8f367c699ae80a7c
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921646"
---
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="fe5e4-102">Sledování vs. Žádné dotazy pro sledování</span><span class="sxs-lookup"><span data-stu-id="fe5e4-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="fe5e4-103">Chování při sledování Určuje, jestli Entity Framework Core v nástroji pro sledování změn budou uchovávat informace o instanci entity.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="fe5e4-104">Pokud je entita sledována, budou všechny změny zjištěné v entitě trvale uchovány v databázi `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="fe5e4-105">Entity Framework Core budou také opravovat navigační vlastnosti mezi entitami získanými z sledovacího dotazu a entit, které byly dříve načteny do instance DbContext.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="fe5e4-106">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="fe5e4-107">Sledování dotazů</span><span class="sxs-lookup"><span data-stu-id="fe5e4-107">Tracking queries</span></span>

<span data-ttu-id="fe5e4-108">Ve výchozím nastavení jsou sledovány dotazy, které vracejí typy entit.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="fe5e4-109">To znamená, že můžete provádět změny těchto instancí entit a nechat tyto změny zachovat `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="fe5e4-110">V následujícím příkladu bude v průběhu `SaveChanges()`aplikace zjištěna změna hodnocení blogů a jejich uchování v databázi.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="fe5e4-111">Žádné dotazy pro sledování</span><span class="sxs-lookup"><span data-stu-id="fe5e4-111">No-tracking queries</span></span>

<span data-ttu-id="fe5e4-112">Žádné sledovací dotazy nejsou užitečné, pokud jsou výsledky použity ve scénáři jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="fe5e4-113">Spouští se rychleji, protože není nutné nastavovat informace o sledování změn.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="fe5e4-114">Jednotlivé dotazy můžete přepínat na možnost bez sledování:</span><span class="sxs-lookup"><span data-stu-id="fe5e4-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="fe5e4-115">Můžete také změnit výchozí chování při sledování na úrovni instance kontextu:</span><span class="sxs-lookup"><span data-stu-id="fe5e4-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="fe5e4-116">Žádné sledovací dotazy stále neprovádějí rozlišení identity v rámci spuštěného dotazu.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-116">No tracking queries still perform identity resolution within the executing query.</span></span> <span data-ttu-id="fe5e4-117">Pokud sada výsledků obsahuje stejnou entitu vícekrát, bude pro každý výskyt v sadě výsledků vrácena stejná instance třídy entity.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="fe5e4-118">Slabé odkazy se však používají k udržení přehledu o entitách, které již byly vráceny.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="fe5e4-119">Pokud předchozí výsledek se stejnou identitou přejde mimo rozsah a spustí se uvolňování paměti, můžete získat novou instanci entity.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="fe5e4-120">Další informace najdete v tématu [Jak funguje dotaz](overview.md).</span><span class="sxs-lookup"><span data-stu-id="fe5e4-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="fe5e4-121">Sledování a projekce</span><span class="sxs-lookup"><span data-stu-id="fe5e4-121">Tracking and projections</span></span>

<span data-ttu-id="fe5e4-122">I v případě, že typ výsledku dotazu není typ entity, pokud výsledek obsahuje typy entit, které budou ve výchozím nastavení sledovány.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="fe5e4-123">V následujícím dotazu, který vrací anonymní typ, se budou sledovat instance `Blog` v sadě výsledků.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=7)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Blog = b,
                Posts = b.Posts.Count()
            });
}
```

<span data-ttu-id="fe5e4-124">Pokud sada výsledků neobsahuje žádné typy entit, sledování se neprovede.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="fe5e4-125">V následujícím dotazu, který vrátí anonymní typ s některými hodnotami z entity (ale žádné instance skutečného typu entity), se neprovede žádné sledování.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Id = b.BlogId,
                Url = b.Url
            });
}
```
