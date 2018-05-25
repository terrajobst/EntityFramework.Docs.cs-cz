---
title: Sledování vs. Ne sledování dotazy – EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="130a6-102">Sledování vs. Ne sledování dotazy</span><span class="sxs-lookup"><span data-stu-id="130a6-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="130a6-103">Ovládací prvky chování sledování, zda Entity Framework Core uchovávat informace o instanci entity v jeho sledování změn.</span><span class="sxs-lookup"><span data-stu-id="130a6-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="130a6-104">Pokud je sledovat entitu, všechny změny v entitě bude natrvalo k databázi během `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="130a6-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="130a6-105">Entity Framework Core bude také opravu up navigační vlastnosti mezi entit, které jsou získány z dotazu sledování a entitami, které byly dříve načteny do DbContext instance.</span><span class="sxs-lookup"><span data-stu-id="130a6-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="130a6-106">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="130a6-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="130a6-107">Dotazy pro sledování</span><span class="sxs-lookup"><span data-stu-id="130a6-107">Tracking queries</span></span>

<span data-ttu-id="130a6-108">Ve výchozím nastavení jsou sledování dotazů, které vrátí typy entit.</span><span class="sxs-lookup"><span data-stu-id="130a6-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="130a6-109">To znamená, můžete provádět změny těchto instancí entit a tyto změny ve uchováván `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="130a6-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="130a6-110">V následujícím příkladu bude zjistil změnu hodnocení blogy a trvalé k databázi během `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="130a6-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="130a6-111">Ne sledování dotazy</span><span class="sxs-lookup"><span data-stu-id="130a6-111">No-tracking queries</span></span>

<span data-ttu-id="130a6-112">Žádné dotazy sledování jsou užitečné, pokud výsledky se používá ve scénáři jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="130a6-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="130a6-113">Jsou rychlejší provést, protože není nutné k instalaci informace o sledování změn.</span><span class="sxs-lookup"><span data-stu-id="130a6-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="130a6-114">Konkrétní dotaz na být ne sledování, můžete zaměnit:</span><span class="sxs-lookup"><span data-stu-id="130a6-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="130a6-115">Můžete také změnit výchozí nastavení sledování chování na úrovni instance kontextu:</span><span class="sxs-lookup"><span data-stu-id="130a6-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="130a6-116">Žádné dotazy sledování i nadále provádět řešení identit v rámci dotaz spustit.</span><span class="sxs-lookup"><span data-stu-id="130a6-116">No tracking queries still perform identity resolution within the excuting query.</span></span> <span data-ttu-id="130a6-117">Pokud sadu výsledků dotazu obsahuje stejné entity vícekrát, stejnou instanci třídy entita se vrátí pro každý výskyt v sadě výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="130a6-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="130a6-118">Slabé odkazy jsou však použít ke sledování entit, které již byly vráceny.</span><span class="sxs-lookup"><span data-stu-id="130a6-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="130a6-119">Pokud předchozí výsledek se stejnou identitou ocitne mimo obor, a uvolňování paměti běží, zobrazí se nové instance entity.</span><span class="sxs-lookup"><span data-stu-id="130a6-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="130a6-120">Další informace najdete v tématu [jak funguje dotazu](overview.md).</span><span class="sxs-lookup"><span data-stu-id="130a6-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="130a6-121">Sledování a projekce</span><span class="sxs-lookup"><span data-stu-id="130a6-121">Tracking and projections</span></span>

<span data-ttu-id="130a6-122">I když výsledný typ dotazu není typ entity, pokud výsledek obsahuje typy entit se stále sledovat ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="130a6-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="130a6-123">V následující dotaz, který vrátí instanci anonymního typu instance `Blog` ve výsledku se bude sledovat sady.</span><span class="sxs-lookup"><span data-stu-id="130a6-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=7)] -->
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

<span data-ttu-id="130a6-124">Pokud sada výsledků neobsahuje žádné typy entit, je provedena bez sledování.</span><span class="sxs-lookup"><span data-stu-id="130a6-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="130a6-125">V následující dotaz, který vrátí anonymní typ s některé hodnoty z entity (ale žádné instance skutečný typ entity), neexistuje žádný sledování provádět.</span><span class="sxs-lookup"><span data-stu-id="130a6-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
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
