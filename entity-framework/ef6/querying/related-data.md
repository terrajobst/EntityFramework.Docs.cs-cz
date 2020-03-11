---
title: Načítají se související entity – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: c359d8d32a88049213fd5e98e99fe49d7e3121a3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417086"
---
# <a name="loading-related-entities"></a><span data-ttu-id="1ed5b-102">Načítají se související entity.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-102">Loading Related Entities</span></span>

<span data-ttu-id="1ed5b-103">Entity Framework podporuje tři způsoby, jak načítat související Eager načítání dat, opožděné načítání a explicitní načítání.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-103">Entity Framework supports three ways to load related data - eager loading, lazy loading and explicit loading.</span></span> <span data-ttu-id="1ed5b-104">Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>

## <a name="eagerly-loading"></a><span data-ttu-id="1ed5b-105">Eagerly načítání</span><span class="sxs-lookup"><span data-stu-id="1ed5b-105">Eagerly Loading</span></span>

<span data-ttu-id="1ed5b-106">Eager načítání je proces, při kterém dotaz pro jeden typ entity také načte související entity jako součást dotazu.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-106">Eager loading is the process whereby a query for one type of entity also loads related entities as part of the query.</span></span> <span data-ttu-id="1ed5b-107">Načítání Eager se dosahuje pomocí metody include.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-107">Eager loading is achieved by use of the Include method.</span></span> <span data-ttu-id="1ed5b-108">Například níže uvedené dotazy načtou Blogy a všechny příspěvky týkající se jednotlivých blogů.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-108">For example, the queries below will load blogs and all the posts related to each blog.</span></span>

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts.
    var blogs1 = context.Blogs
                        .Include(b => b.Posts)
                        .ToList();

    // Load one blog and its related posts.
    var blog1 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include(b => b.Posts)
                       .FirstOrDefault();

    // Load all blogs and related posts
    // using a string to specify the relationship.
    var blogs2 = context.Blogs
                        .Include("Posts")
                        .ToList();

    // Load one blog and its related posts
    // using a string to specify the relationship.
    var blog2 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include("Posts")
                       .FirstOrDefault();
}
```

> [!NOTE]
> <span data-ttu-id="1ed5b-109">Include je rozšiřující metoda v oboru názvů System. data. entity, takže se ujistěte, že používáte tento obor názvů.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-109">Include is an extension method in the System.Data.Entity namespace so make sure you are using that namespace.</span></span>

### <a name="eagerly-loading-multiple-levels"></a><span data-ttu-id="1ed5b-110">Eagerly načítání více úrovní</span><span class="sxs-lookup"><span data-stu-id="1ed5b-110">Eagerly loading multiple levels</span></span>

<span data-ttu-id="1ed5b-111">Je také možné eagerly načíst více úrovní souvisejících entit.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-111">It is also possible to eagerly load multiple levels of related entities.</span></span> <span data-ttu-id="1ed5b-112">Níže uvedené dotazy ukazují příklady toho, jak to udělat pro vlastnosti kolekce i navigační navigace.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-112">The queries below show examples of how to do this for both collection and reference navigation properties.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments.
    var blogs1 = context.Blogs
                        .Include(b => b.Posts.Select(p => p.Comments))
                        .ToList();

    // Load all users, their related profiles, and related avatar.
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships.
    var blogs2 = context.Blogs
                        .Include("Posts.Comments")
                        .ToList();

    // Load all users, their related profiles, and related avatar  
    // using a string to specify the relationships.
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

> [!NOTE]
> <span data-ttu-id="1ed5b-113">V tuto chvíli není možné filtrovat, které související entity se načítají.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-113">It is not currently possible to filter which related entities are loaded.</span></span> <span data-ttu-id="1ed5b-114">Zahrnutí se vždycky přenese do všech souvisejících entit.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-114">Include will always bring in all related entities.</span></span>  

## <a name="lazy-loading"></a><span data-ttu-id="1ed5b-115">Opožděné načítání</span><span class="sxs-lookup"><span data-stu-id="1ed5b-115">Lazy Loading</span></span>

<span data-ttu-id="1ed5b-116">Opožděné načítání je proces, při kterém je entita nebo kolekce entit automaticky načtena z databáze při prvním otevření vlastnosti odkazující na entitu nebo entity.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-116">Lazy loading is the process whereby an entity or collection of entities is automatically loaded from the database the first time that a property referring to the entity/entities is accessed.</span></span> <span data-ttu-id="1ed5b-117">Při použití typů entit POCO je dosaženo opožděné načítání vytvořením instancí odvozených typů proxy a následným přepsáním virtuálních vlastností pro přidání zavěšení zatížení.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-117">When using POCO entity types, lazy loading is achieved by creating instances of derived proxy types and then overriding virtual properties to add the loading hook.</span></span> <span data-ttu-id="1ed5b-118">Pokud například používáte třídu entity blogu definovanou níže, související příspěvky se načtou při prvním otevření navigační vlastnosti příspěvky:</span><span class="sxs-lookup"><span data-stu-id="1ed5b-118">For example, when using the Blog entity class defined below, the related Posts will be loaded the first time the Posts navigation property is accessed:</span></span>

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

### <a name="turn-lazy-loading-off-for-serialization"></a><span data-ttu-id="1ed5b-119">Zapnout opožděné načítání pro serializaci</span><span class="sxs-lookup"><span data-stu-id="1ed5b-119">Turn lazy loading off for serialization</span></span>

<span data-ttu-id="1ed5b-120">Opožděné načítání a serializace se dobře nespojují. Pokud nemusíte pozor, můžete ukončit dotazování pro celou databázi, a to jenom v případě, že je povolené opožděné načítání.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-120">Lazy loading and serialization don’t mix well, and if you aren’t careful you can end up querying for your entire database just because lazy loading is enabled.</span></span> <span data-ttu-id="1ed5b-121">Většina serializátorů funguje při přístupu k jednotlivým vlastnostem instance typu.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-121">Most serializers work by accessing each property on an instance of a type.</span></span> <span data-ttu-id="1ed5b-122">Přístup k vlastnostem aktivuje opožděné načítání, takže se další entity získají serializovat.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-122">Property access triggers lazy loading, so more entities get serialized.</span></span> <span data-ttu-id="1ed5b-123">K těmto vlastnostem entit se dostanete a načtou se i další entity.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-123">On those entities properties are accessed, and even more entities are loaded.</span></span> <span data-ttu-id="1ed5b-124">Před serializací entity je dobrým zvykem zapnout opožděné načítání.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-124">It’s a good practice to turn lazy loading off before you serialize an entity.</span></span> <span data-ttu-id="1ed5b-125">Následující části vysvětlují, jak to udělat.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-125">The following sections show how to do this.</span></span>

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a><span data-ttu-id="1ed5b-126">Vypnutí opožděného načítání pro konkrétní navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="1ed5b-126">Turning off lazy loading for specific navigation properties</span></span>

<span data-ttu-id="1ed5b-127">Opožděné načítání kolekce příspěvků lze vypnout tím, že vlastnost posts není virtuální:</span><span class="sxs-lookup"><span data-stu-id="1ed5b-127">Lazy loading of the Posts collection can be turned off by making the Posts property non-virtual:</span></span>

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

<span data-ttu-id="1ed5b-128">Načítání kolekce příspěvků lze stále dosáhnout pomocí Eager načítání (viz *eagerly* Load výše) nebo metody Load (viz *explicitní načtení* níže).</span><span class="sxs-lookup"><span data-stu-id="1ed5b-128">Loading of the Posts collection can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>

### <a name="turn-off-lazy-loading-for-all-entities"></a><span data-ttu-id="1ed5b-129">Vypnout opožděné načítání pro všechny entity</span><span class="sxs-lookup"><span data-stu-id="1ed5b-129">Turn off lazy loading for all entities</span></span>

<span data-ttu-id="1ed5b-130">Opožděné načítání lze vypnout pro všechny entity v kontextu nastavením příznaku na vlastnost konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-130">Lazy loading can be turned off for all entities in the context by setting a flag on the Configuration property.</span></span> <span data-ttu-id="1ed5b-131">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1ed5b-131">For example:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```

<span data-ttu-id="1ed5b-132">Načítání souvisejících entit se stále může dosáhnout pomocí Eager načítání (viz *eagerly* Load výše) nebo metodou Load (viz *explicitní načítání* níže).</span><span class="sxs-lookup"><span data-stu-id="1ed5b-132">Loading of related entities can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>

## <a name="explicitly-loading"></a><span data-ttu-id="1ed5b-133">Explicitní načítání</span><span class="sxs-lookup"><span data-stu-id="1ed5b-133">Explicitly Loading</span></span>

<span data-ttu-id="1ed5b-134">I v případě, že je opožděné načítání zakázané, je stále možné laxně vytvářená načíst související entity, ale je nutné provést explicitní volání.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-134">Even with lazy loading disabled it is still possible to lazily load related entities, but it must be done with an explicit call.</span></span> <span data-ttu-id="1ed5b-135">K tomu slouží metoda Load pro položku související entity.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-135">To do so you use the Load method on the related entity’s entry.</span></span> <span data-ttu-id="1ed5b-136">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1ed5b-136">For example:</span></span>

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post.
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string.
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog.
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog
    // using a string to specify the relationship.
    context.Entry(blog).Collection("Posts").Load();
}
```

> [!NOTE]
> <span data-ttu-id="1ed5b-137">Metoda reference by měla být použita, pokud má entita navigační vlastnost pro jinou jednu entitu.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-137">The Reference method should be used when an entity has a navigation property to another single entity.</span></span> <span data-ttu-id="1ed5b-138">Na druhé straně by se měla použít metoda kolekce, pokud má entita navigační vlastnost pro kolekci jiných entit.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-138">On the other hand, the Collection method should be used when an entity has a navigation property to a collection of other entities.</span></span>

### <a name="applying-filters-when-explicitly-loading-related-entities"></a><span data-ttu-id="1ed5b-139">Použití filtrů při explicitním načítání souvisejících entit</span><span class="sxs-lookup"><span data-stu-id="1ed5b-139">Applying filters when explicitly loading related entities</span></span>

<span data-ttu-id="1ed5b-140">Metoda dotazu poskytuje přístup k podkladovým dotazům, které Entity Framework použijí při načítání souvisejících entit.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-140">The Query method provides access to the underlying query that Entity Framework will use when loading related entities.</span></span> <span data-ttu-id="1ed5b-141">Pak můžete použít LINQ k aplikování filtrů na dotaz před jeho provedením voláním metody rozšíření LINQ, jako je ToList –, Load atd. Metoda dotazu může být použita s oběma vlastnostmi odkazu i navigace v kolekci, ale je nejužitečnější pro kolekce, kde je lze použít pro načtení pouze části kolekce.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-141">You can then use LINQ to apply filters to the query before executing it with a call to a LINQ extension method such as ToList, Load, etc. The Query method can be used with both reference and collection navigation properties but is most useful for collections where it can be used to load only part of the collection.</span></span> <span data-ttu-id="1ed5b-142">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1ed5b-142">For example:</span></span>

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog.
    context.Entry(blog)
           .Collection(b => b.Posts)
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog
    // using a string to specify the relationship.
    context.Entry(blog)
           .Collection("Posts")
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();
}
```

<span data-ttu-id="1ed5b-143">Při použití metody dotazu obvykle je nejlepší vypnout opožděné načítání pro navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-143">When using the Query method it is usually best to turn off lazy loading for the navigation property.</span></span> <span data-ttu-id="1ed5b-144">Důvodem je, že v opačném případě může být celá kolekce automaticky načtena mechanismem opožděného načítání buď před, nebo po provedení filtrovaného dotazu.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-144">This is because otherwise the entire collection may get loaded automatically by the lazy loading mechanism either before or after the filtered query has been executed.</span></span>

> [!NOTE]
> <span data-ttu-id="1ed5b-145">Zatímco relaci lze zadat jako řetězec namísto lambda výrazu, vrácený parametr IQueryable není obecný, pokud je použit řetězec, a proto je metoda přetypování obvykle potřebná předtím, než je možné s ní provádět cokoli užitečné.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-145">While the relationship can be specified as a string instead of a lambda expression, the returned IQueryable is not generic when a string is used and so the Cast method is usually needed before anything useful can be done with it.</span></span>

## <a name="using-query-to-count-related-entities-without-loading-them"></a><span data-ttu-id="1ed5b-146">Použití dotazu k počítání souvisejících entit bez jejich načtení</span><span class="sxs-lookup"><span data-stu-id="1ed5b-146">Using Query to count related entities without loading them</span></span>

<span data-ttu-id="1ed5b-147">Někdy je užitečné zjistit, kolik entit souvisí s jinou entitou v databázi, aniž by to mělo za následek nasazování všech těchto entit.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-147">Sometimes it is useful to know how many entities are related to another entity in the database without actually incurring the cost of loading all those entities.</span></span> <span data-ttu-id="1ed5b-148">K tomu lze použít metodu dotazu s metodou LINQ Count.</span><span class="sxs-lookup"><span data-stu-id="1ed5b-148">The Query method with the LINQ Count method can be used to do this.</span></span> <span data-ttu-id="1ed5b-149">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1ed5b-149">For example:</span></span>

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has.
    var postCount = context.Entry(blog)
                           .Collection(b => b.Posts)
                           .Query()
                           .Count();
}
```
