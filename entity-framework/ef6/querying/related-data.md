---
title: Načítají se související entity - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: 091493f60c732573ca63adb8c65bc28606453d34
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995922"
---
# <a name="loading-related-entities"></a><span data-ttu-id="43b32-102">Načítají se související entity</span><span class="sxs-lookup"><span data-stu-id="43b32-102">Loading Related Entities</span></span>
<span data-ttu-id="43b32-103">Entity Framework podporuje tři způsoby, jak načíst související data - eager načítání, opožděné načtení a explicitní načtení.</span><span class="sxs-lookup"><span data-stu-id="43b32-103">Entity Framework supports three ways to load related data - eager loading, lazy loading and explicit loading.</span></span> <span data-ttu-id="43b32-104">Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.</span><span class="sxs-lookup"><span data-stu-id="43b32-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="eagerly-loading"></a><span data-ttu-id="43b32-105">Například načítání</span><span class="sxs-lookup"><span data-stu-id="43b32-105">Eagerly Loading</span></span>  

<span data-ttu-id="43b32-106">Předběžné načítání je proces, kterým dotazu pro jeden typ entity se také načtou související entity jako součást dotazu.</span><span class="sxs-lookup"><span data-stu-id="43b32-106">Eager loading is the process whereby a query for one type of entity also loads related entities as part of the query.</span></span> <span data-ttu-id="43b32-107">Předběžné načítání je dosaženo pomocí metody Include.</span><span class="sxs-lookup"><span data-stu-id="43b32-107">Eager loading is achieved by use of the Include method.</span></span> <span data-ttu-id="43b32-108">Například níže uvedené dotazy se načte blogové příspěvky a všechny příspěvky vztahující se k blogů.</span><span class="sxs-lookup"><span data-stu-id="43b32-108">For example, the queries below will load blogs and all the posts related to each blog.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                          .Include(b => b.Posts)
                          .ToList();

    // Load one blogs and its related posts
    var blog1 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include(b => b.Posts)
                        .FirstOrDefault();

    // Load all blogs and related posts  
    // using a string to specify the relationship
    var blogs2 = context.Blogs
                          .Include("Posts")
                          .ToList();

    // Load one blog and its related posts  
    // using a string to specify the relationship
    var blog2 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include("Posts")
                        .FirstOrDefault();
}
```  

<span data-ttu-id="43b32-109">Všimněte si, zahrnout je rozšiřující metodu v oboru názvů System.Data.Entity proto se ujistěte, že používáte tento obor názvů.</span><span class="sxs-lookup"><span data-stu-id="43b32-109">Note that Include is an extension method in the System.Data.Entity namespace so make sure you are using that namespace.</span></span>  

### <a name="eagerly-loading-multiple-levels"></a><span data-ttu-id="43b32-110">Například načítání více úrovní</span><span class="sxs-lookup"><span data-stu-id="43b32-110">Eagerly loading multiple levels</span></span>  

<span data-ttu-id="43b32-111">Je také možné například načíst několik úrovní související entity.</span><span class="sxs-lookup"><span data-stu-id="43b32-111">It is also possible to eagerly load multiple levels of related entities.</span></span> <span data-ttu-id="43b32-112">Níže uvedené dotazy ukazují příklady toho, jak to provést u kolekce a odkaz na navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="43b32-112">The queries below show examples of how to do this for both collection and reference navigation properties.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                       .Include(b => b.Posts.Select(p => p.Comments))
                       .ToList();

    // Load all users their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                       .Include("Posts.Comments")
                       .ToList();

    // Load all users their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

<span data-ttu-id="43b32-113">Všimněte si, že není aktuálně můžete provést filtrování souvisejících entit, které jsou načteny.</span><span class="sxs-lookup"><span data-stu-id="43b32-113">Note that it is not currently possible to filter which related entities are loaded.</span></span> <span data-ttu-id="43b32-114">Zahrnout bude vždy umožňuje přinést si všechny související entity.</span><span class="sxs-lookup"><span data-stu-id="43b32-114">Include will always bring in all related entities.</span></span>  

## <a name="lazy-loading"></a><span data-ttu-id="43b32-115">Opožděné načtení</span><span class="sxs-lookup"><span data-stu-id="43b32-115">Lazy Loading</span></span>  

<span data-ttu-id="43b32-116">Opožděné načtení je proces, kterým entitu nebo kolekci entit se automaticky načtou z databáze při prvním přístupu k vlastnosti odkazující na entity nebo entities.</span><span class="sxs-lookup"><span data-stu-id="43b32-116">Lazy loading is the process whereby an entity or collection of entities is automatically loaded from the database the first time that a property referring to the entity/entities is accessed.</span></span> <span data-ttu-id="43b32-117">Při použití typů entit POCO, opožděné načtení se dosahuje prostřednictvím vytváření instancí typů odvozených proxy serveru a potom přepsáním virtuální vlastnosti přidáte hook načítání.</span><span class="sxs-lookup"><span data-stu-id="43b32-117">When using POCO entity types, lazy loading is achieved by creating instances of derived proxy types and then overriding virtual properties to add the loading hook.</span></span> <span data-ttu-id="43b32-118">Například pokud používáte třídu entity blogu definovaná níže, souvisejících příspěvků se načtou při prvním příspěvky vlastnost navigace pracuje:</span><span class="sxs-lookup"><span data-stu-id="43b32-118">For example, when using the Blog entity class defined below, the related Posts will be loaded the first time the Posts navigation property is accessed:</span></span>  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public virtual ICollection<Post> Posts { get; set; }  
}
```  

### <a name="turn-lazy-loading-off-for-serialization"></a><span data-ttu-id="43b32-119">Zapnout opožděné načtení vypnout pro serializaci</span><span class="sxs-lookup"><span data-stu-id="43b32-119">Turn lazy loading off for serialization</span></span>  

<span data-ttu-id="43b32-120">Serializace a opožděné načtení Nekombinujte dobře a nevyloučíte můžete skončit dotazování pro celou databázi pouze z důvodu opožděné načtení je povolená.</span><span class="sxs-lookup"><span data-stu-id="43b32-120">Lazy loading and serialization don’t mix well, and if you aren’t careful you can end up querying for your entire database just because lazy loading is enabled.</span></span> <span data-ttu-id="43b32-121">Většina serializátory práci díky přístupu do každou vlastnost v instanci typu.</span><span class="sxs-lookup"><span data-stu-id="43b32-121">Most serializers work by accessing each property on an instance of a type.</span></span> <span data-ttu-id="43b32-122">Přístup k vlastnostem aktivuje opožděné načtení, tak se serializují dalších entit.</span><span class="sxs-lookup"><span data-stu-id="43b32-122">Property access triggers lazy loading, so more entities get serialized.</span></span> <span data-ttu-id="43b32-123">S těmito entitami přístup k vlastnostem a ještě více entit se načtou.</span><span class="sxs-lookup"><span data-stu-id="43b32-123">On those entities properties are accessed, and even more entities are loaded.</span></span> <span data-ttu-id="43b32-124">Je vhodné zapnout opožděné načtení vypnout před serializovat entity.</span><span class="sxs-lookup"><span data-stu-id="43b32-124">It’s a good practice to turn lazy loading off before you serialize an entity.</span></span> <span data-ttu-id="43b32-125">Následující části vysvětlují, jak to udělat.</span><span class="sxs-lookup"><span data-stu-id="43b32-125">The following sections show how to do this.</span></span>  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a><span data-ttu-id="43b32-126">Vypnutí opožděné načtení pro konkrétní navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="43b32-126">Turning off lazy loading for specific navigation properties</span></span>  

<span data-ttu-id="43b32-127">Opožděné načtení kolekce příspěvky můžete vypnout tak, že vlastnost příspěvky nevirtuální:</span><span class="sxs-lookup"><span data-stu-id="43b32-127">Lazy loading of the Posts collection can be turned off by making the Posts property non-virtual:</span></span>  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public ICollection<Post> Posts { get; set; }  
}
```  

<span data-ttu-id="43b32-128">Načítání příspěvků kolekce stále lze dosáhnout pomocí předběžné načítání (naleznete v tématu *například načítání* výše) nebo metodu načtení (naleznete v tématu *explicitně načítání* níže).</span><span class="sxs-lookup"><span data-stu-id="43b32-128">Loading of the Posts collection can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

### <a name="turn-off-lazy-loading-for-all-entities"></a><span data-ttu-id="43b32-129">Vypnout opožděné načtení pro všechny entity</span><span class="sxs-lookup"><span data-stu-id="43b32-129">Turn off lazy loading for all entities</span></span>  

<span data-ttu-id="43b32-130">Opožděné načtení může být vypnuté pro všechny entity v kontextu nastavením příznaku v konfigurační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="43b32-130">Lazy loading can be turned off for all entities in the context by setting a flag on the Configuration property.</span></span> <span data-ttu-id="43b32-131">Příklad:</span><span class="sxs-lookup"><span data-stu-id="43b32-131">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

<span data-ttu-id="43b32-132">Načítání souvisejících entit stále lze dosáhnout pomocí předběžné načítání (naleznete v tématu *například načítání* výše) nebo metodu načtení (naleznete v tématu *explicitně načítání* níže).</span><span class="sxs-lookup"><span data-stu-id="43b32-132">Loading of related entities can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

## <a name="explicitly-loading"></a><span data-ttu-id="43b32-133">Explicitně načítání</span><span class="sxs-lookup"><span data-stu-id="43b32-133">Explicitly Loading</span></span>  

<span data-ttu-id="43b32-134">I přes opožděné načítání je zakázáno je stále možné laxně načtení souvisejících entit, ale je nutné provést pomocí explicitní volání konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="43b32-134">Even with lazy loading disabled it is still possible to lazily load related entities, but it must be done with an explicit call.</span></span> <span data-ttu-id="43b32-135">K tomu použít metodu zatížení u související entity položky.</span><span class="sxs-lookup"><span data-stu-id="43b32-135">To do so you use the Load method on the related entity’s entry.</span></span> <span data-ttu-id="43b32-136">Příklad:</span><span class="sxs-lookup"><span data-stu-id="43b32-136">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string  
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog  
    // using a string to specify the relationship
    context.Entry(blog).Collection("Posts").Load();
}
```  

<span data-ttu-id="43b32-137">Všimněte si, že metodu odkaz by měl být pokud má entita navigační vlastnost pro další jednu entitu.</span><span class="sxs-lookup"><span data-stu-id="43b32-137">Note that the Reference method should be used when an entity has a navigation property to another single entity.</span></span> <span data-ttu-id="43b32-138">Metodu kolekce na druhé straně by měl použít, pokud má entita navigační vlastnost kolekce jiných entit.</span><span class="sxs-lookup"><span data-stu-id="43b32-138">On the other hand, the Collection method should be used when an entity has a navigation property to a collection of other entities.</span></span>  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a><span data-ttu-id="43b32-139">Použití filtrů při načítání explicitně související entity</span><span class="sxs-lookup"><span data-stu-id="43b32-139">Applying filters when explicitly loading related entities</span></span>  

<span data-ttu-id="43b32-140">Metoda dotazu poskytuje přístup k základní dotaz, který Entity Framework bude používat při načítání souvisejících entit.</span><span class="sxs-lookup"><span data-stu-id="43b32-140">The Query method provides access to the underlying query that Entity Framework will use when loading related entities.</span></span> <span data-ttu-id="43b32-141">Potom můžete LINQ a nastavte filtry pro dotaz před spuštěním pomocí volání metody rozšíření LINQ jako je například ToList zatížení, atd. Metodu dotazu je možné použít s vlastnostmi navigační odkaz a kolekce, ale je zvláště užitečná pro kolekce, ve kterém je možné načíst pouze část shromažďování.</span><span class="sxs-lookup"><span data-stu-id="43b32-141">You can then use LINQ to apply filters to the query before executing it with a call to a LINQ extension method such as ToList, Load, etc. The Query method can be used with both reference and collection navigation properties but is most useful for collections where it can be used to load only part of the collection.</span></span> <span data-ttu-id="43b32-142">Příklad:</span><span class="sxs-lookup"><span data-stu-id="43b32-142">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
        .Collection("Posts")
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  

<span data-ttu-id="43b32-143">Při použití metody dotazu je obvykle nejlepší volbou, chcete-li vypnout opožděné načtení pro navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="43b32-143">When using the Query method it is usually best to turn off lazy loading for the navigation property.</span></span> <span data-ttu-id="43b32-144">Je to proto jinak celou kolekci mohou získat načteny automaticky opožděné načtení mechanismem před nebo po provedení filtrovaného dotazu.</span><span class="sxs-lookup"><span data-stu-id="43b32-144">This is because otherwise the entire collection may get loaded automatically by the lazy loading mechanism either before or after the filtered query has been executed.</span></span>  

<span data-ttu-id="43b32-145">Všimněte si, že během relace můžete zadat jako řetězec namísto výrazu lambda, vrácený typ IQueryable není obecná při řetězec se používá, a proto metodu přetypování je obvykle potřeba před nic užitečného můžete s ním dá dělat.</span><span class="sxs-lookup"><span data-stu-id="43b32-145">Note that while the relationship can be specified as a string instead of a lambda expression, the returned IQueryable is not generic when a string is used and so the Cast method is usually needed before anything useful can be done with it.</span></span>  

## <a name="using-query-to-count-related-entities-without-loading-them"></a><span data-ttu-id="43b32-146">Pomocí dotazu na počet souvisejících entit bez jejich načtení</span><span class="sxs-lookup"><span data-stu-id="43b32-146">Using Query to count related entities without loading them</span></span>  

<span data-ttu-id="43b32-147">Někdy je užitečné vědět, kolik entity se vztahují na jinou entitu v databázi bez ve skutečnosti by tím narůstaly náklady na načtení těchto entit.</span><span class="sxs-lookup"><span data-stu-id="43b32-147">Sometimes it is useful to know how many entities are related to another entity in the database without actually incurring the cost of loading all those entities.</span></span> <span data-ttu-id="43b32-148">K tomu je možné metodu dotazu pomocí LINQ Count – metoda.</span><span class="sxs-lookup"><span data-stu-id="43b32-148">The Query method with the LINQ Count method can be used to do this.</span></span> <span data-ttu-id="43b32-149">Příklad:</span><span class="sxs-lookup"><span data-stu-id="43b32-149">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has  
    var postCount = context.Entry(blog)
                          .Collection(b => b.Posts)
                          .Query()
                          .Count();
}
```  
