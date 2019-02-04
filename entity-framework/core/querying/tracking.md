---
title: Sledování vs. Sledování bez dotazy – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: 6c5d516fcb3950ae168860029660e1b1061546b8
ms.sourcegitcommit: 159c2e9afed7745e7512730ffffaf154bcf2ff4a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668775"
---
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="90879-102">Sledování vs. Sledování bez dotazy</span><span class="sxs-lookup"><span data-stu-id="90879-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="90879-103">Sledování chování ovládacích prvků, zda Entity Framework Core uchovávat informace o instanci entity v jeho sledování změn.</span><span class="sxs-lookup"><span data-stu-id="90879-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="90879-104">Pokud je sledována entity, všechny změny v entitě se ukládají do databáze během `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="90879-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="90879-105">Entity Framework Core se také opravit navigačních vlastností mezi entitami, které jsou získávány z dotazu sledování a entity, které byly dříve načtena do DbContext instance.</span><span class="sxs-lookup"><span data-stu-id="90879-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="90879-106">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="90879-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="90879-107">Sledování dotazy</span><span class="sxs-lookup"><span data-stu-id="90879-107">Tracking queries</span></span>

<span data-ttu-id="90879-108">Ve výchozím nastavení jsou sledování dotazy, které vracejí typy entit.</span><span class="sxs-lookup"><span data-stu-id="90879-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="90879-109">To znamená, že provedete změny u těch instancí entit a tyto změny nebyla uložena ve `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="90879-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="90879-110">V následujícím příkladu, změna hodnocení blogy zjištěna a ukládají do databáze během `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="90879-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="90879-111">Sledování bez dotazy</span><span class="sxs-lookup"><span data-stu-id="90879-111">No-tracking queries</span></span>

<span data-ttu-id="90879-112">Žádné sledování dotazy jsou užitečné, když výsledky se používají ve scénáři jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="90879-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="90879-113">Jsou rychlejší provést, protože není nutné k instalaci informace o sledování změn.</span><span class="sxs-lookup"><span data-stu-id="90879-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="90879-114">Jednotlivé dotazy se no-sledování, můžete zaměnit:</span><span class="sxs-lookup"><span data-stu-id="90879-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="90879-115">Můžete také změnit výchozí chování na úrovni instance kontextu sledování:</span><span class="sxs-lookup"><span data-stu-id="90879-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="90879-116">Žádné dotazy sledování stále zlepšovat řešení identit v rámci provádění dotazu.</span><span class="sxs-lookup"><span data-stu-id="90879-116">No tracking queries still perform identity resolution within the executing query.</span></span> <span data-ttu-id="90879-117">Pokud sada výsledků obsahuje více než jednou stejnou entitu, vrátí se stejnou instanci třídy entity u každého výskytu v sadě výsledků.</span><span class="sxs-lookup"><span data-stu-id="90879-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="90879-118">Slabé odkazy se ale používají ke sledování entit, které již byly vráceny.</span><span class="sxs-lookup"><span data-stu-id="90879-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="90879-119">Pokud předchozí výsledek se stejnou identitou dostane mimo rozsah a uvolňování paměti běží, může se zobrazit nová instance entity.</span><span class="sxs-lookup"><span data-stu-id="90879-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="90879-120">Další informace najdete v tématu [jak funguje dotaz](overview.md).</span><span class="sxs-lookup"><span data-stu-id="90879-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="90879-121">Sledování a projekce</span><span class="sxs-lookup"><span data-stu-id="90879-121">Tracking and projections</span></span>

<span data-ttu-id="90879-122">I v případě, že typ výsledku dotazu není typ entity, pokud výsledek obsahuje typy entit se pořád sledovat ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="90879-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="90879-123">V následující dotaz, který vrátí anonymní typ instance `Blog` ve výsledku bude sledovat sadu.</span><span class="sxs-lookup"><span data-stu-id="90879-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

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

<span data-ttu-id="90879-124">Pokud sada výsledků neobsahuje žádné typy entit, se neprovádí žádné sledování.</span><span class="sxs-lookup"><span data-stu-id="90879-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="90879-125">V následující dotaz, který vrátí anonymní typ s některými z hodnot z entity (ale bez instance typu skutečné entity), neexistuje žádné sledování provádět.</span><span class="sxs-lookup"><span data-stu-id="90879-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

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
