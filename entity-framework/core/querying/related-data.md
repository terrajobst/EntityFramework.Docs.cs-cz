---
title: Načítají se související data – EF Core.
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 915aaa41beb495a046f2d6260e9c3b174d5f3031
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417675"
---
# <a name="loading-related-data"></a><span data-ttu-id="c240d-102">Načítání souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="c240d-102">Loading Related Data</span></span>

<span data-ttu-id="c240d-103">Entity Framework Core umožňuje v modelu použít navigační vlastnosti pro načtení souvisejících entit.</span><span class="sxs-lookup"><span data-stu-id="c240d-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="c240d-104">Existují tři běžné vzory O/RM, které slouží k načtení souvisejících dat.</span><span class="sxs-lookup"><span data-stu-id="c240d-104">There are three common O/RM patterns used to load related data.</span></span>

* <span data-ttu-id="c240d-105">**Eager načítání** znamená, že související data jsou načtena z databáze jako součást počátečního dotazu.</span><span class="sxs-lookup"><span data-stu-id="c240d-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="c240d-106">**Explicitní načítání** znamená, že související data jsou explicitně načtena z databáze později.</span><span class="sxs-lookup"><span data-stu-id="c240d-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="c240d-107">**Opožděné načítání** znamená, že související data jsou transparentně načítána z databáze, pokud je k ní přistupovaná vlastnost navigace.</span><span class="sxs-lookup"><span data-stu-id="c240d-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="c240d-108">[Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) tohoto článku můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="c240d-108">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="c240d-109">Eager načítání</span><span class="sxs-lookup"><span data-stu-id="c240d-109">Eager loading</span></span>

<span data-ttu-id="c240d-110">Pomocí metody `Include` můžete určit související data, která se mají zahrnout do výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="c240d-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="c240d-111">V následujícím příkladu budou Blogy, které jsou vráceny ve výsledcích, obsahovat jejich `Posts` vlastnost naplněná souvisejícími příspěvky.</span><span class="sxs-lookup"><span data-stu-id="c240d-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="c240d-112">Entity Framework Core bude automaticky opravovat navigační vlastnosti pro všechny další entity, které byly dříve načteny do instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="c240d-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="c240d-113">Takže i když nebudete explicitně vkládat data pro navigační vlastnost, může být tato vlastnost i nadále naplněna, pokud byly některé nebo všechny související entity dříve načteny.</span><span class="sxs-lookup"><span data-stu-id="c240d-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

<span data-ttu-id="c240d-114">V jednom dotazu můžete zahrnout související data z několika vztahů.</span><span class="sxs-lookup"><span data-stu-id="c240d-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="c240d-115">Zahrnutí více úrovní</span><span class="sxs-lookup"><span data-stu-id="c240d-115">Including multiple levels</span></span>

<span data-ttu-id="c240d-116">Pomocí metody `ThenInclude` můžete procházet hierarchii a zahrnout do nich více úrovní souvisejících dat.</span><span class="sxs-lookup"><span data-stu-id="c240d-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="c240d-117">Následující příklad načte všechny Blogy, jejich související příspěvky a autora každého příspěvku.</span><span class="sxs-lookup"><span data-stu-id="c240d-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

<span data-ttu-id="c240d-118">Můžete zřetězit více volání `ThenInclude`, abyste mohli pokračovat, včetně dalších úrovní souvisejících dat.</span><span class="sxs-lookup"><span data-stu-id="c240d-118">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="c240d-119">To všechno můžete zkombinovat, chcete-li zahrnout související data z více úrovní a více kořenů ve stejném dotazu.</span><span class="sxs-lookup"><span data-stu-id="c240d-119">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="c240d-120">Možná budete chtít zahrnout několik souvisejících entit pro jednu z zahrnutých entit.</span><span class="sxs-lookup"><span data-stu-id="c240d-120">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="c240d-121">Při dotazování `Blogs`například zahrnete `Posts` a pak chcete zahrnout `Author` i `Tags` `Posts`.</span><span class="sxs-lookup"><span data-stu-id="c240d-121">For example, when querying `Blogs`, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="c240d-122">Chcete-li to provést, je třeba zadat každou cestu zahrnutí počínaje kořenovým adresářem.</span><span class="sxs-lookup"><span data-stu-id="c240d-122">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="c240d-123">Například `Blog -> Posts -> Author` a `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="c240d-123">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="c240d-124">To neznamená, že budete mít redundantní spojení. ve většině případů bude EF konsolidovat spojení při generování SQL.</span><span class="sxs-lookup"><span data-stu-id="c240d-124">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

> [!CAUTION]
> <span data-ttu-id="c240d-125">Vzhledem k tomu, že verze 3.0.0, každá `Include` způsobí přidání dalších připojení k dotazům SQL vytvořeným relačními zprostředkovateli, zatímco předchozí verze vygenerovaly další dotazy SQL.</span><span class="sxs-lookup"><span data-stu-id="c240d-125">Since version 3.0.0, each `Include` will cause an additional JOIN to be added to SQL queries produced by relational providers, whereas previous versions generated additional SQL queries.</span></span> <span data-ttu-id="c240d-126">To může významně změnit výkon dotazů pro lepší nebo horší.</span><span class="sxs-lookup"><span data-stu-id="c240d-126">This can significantly change the performance of your queries, for better or worse.</span></span> <span data-ttu-id="c240d-127">Konkrétně dotazy LINQ s větším počtem `Include`ch operátorů může být nutné rozdělit do několika samostatných dotazů LINQ, aby se zabránilo problému s vyvýbuchou kartézském.</span><span class="sxs-lookup"><span data-stu-id="c240d-127">In particular, LINQ queries with an exceedingly high number of `Include` operators may need to be broken down into multiple separate LINQ queries in order to avoid the cartesian explosion problem.</span></span>

### <a name="include-on-derived-types"></a><span data-ttu-id="c240d-128">Zahrnout do odvozených typů</span><span class="sxs-lookup"><span data-stu-id="c240d-128">Include on derived types</span></span>

<span data-ttu-id="c240d-129">Můžete zahrnout související data z navigace definovaná pouze pro odvozený typ pomocí `Include` a `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="c240d-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span>

<span data-ttu-id="c240d-130">S ohledem na následující model:</span><span class="sxs-lookup"><span data-stu-id="c240d-130">Given the following model:</span></span>

```csharp
public class SchoolContext : DbContext
{
    public DbSet<Person> People { get; set; }
    public DbSet<School> Schools { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
    }
}

public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class Student : Person
{
    public School School { get; set; }
}

public class School
{
    public int Id { get; set; }
    public string Name { get; set; }

    public List<Student> Students { get; set; }
}
```

<span data-ttu-id="c240d-131">Obsah `School` navigace všech lidí, kteří Studenté můžou načítat eagerly pomocí několika vzorů:</span><span class="sxs-lookup"><span data-stu-id="c240d-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

* <span data-ttu-id="c240d-132">použití přetypování</span><span class="sxs-lookup"><span data-stu-id="c240d-132">using cast</span></span>

  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

* <span data-ttu-id="c240d-133">použití operátoru `as`</span><span class="sxs-lookup"><span data-stu-id="c240d-133">using `as` operator</span></span>

  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

* <span data-ttu-id="c240d-134">použití přetížení `Include`, které přebírá parametr typu `string`</span><span class="sxs-lookup"><span data-stu-id="c240d-134">using overload of `Include` that takes parameter of type `string`</span></span>

  ```csharp
  context.People.Include("School").ToList()
  ```

## <a name="explicit-loading"></a><span data-ttu-id="c240d-135">explicitní načítání</span><span class="sxs-lookup"><span data-stu-id="c240d-135">Explicit loading</span></span>

<span data-ttu-id="c240d-136">Můžete explicitně načíst navigační vlastnost prostřednictvím rozhraní `DbContext.Entry(...)` API.</span><span class="sxs-lookup"><span data-stu-id="c240d-136">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="c240d-137">Můžete také explicitně načíst vlastnost navigace spuštěním samostatného dotazu, který vrací související entity.</span><span class="sxs-lookup"><span data-stu-id="c240d-137">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="c240d-138">Pokud je povoleno sledování změn, pak při načítání entity EF Core automaticky nastaví navigační vlastnosti nově načtených entity, aby odkazovaly na jakékoli již načtené entity, a nastavili navigační vlastnosti již načtených entit tak, aby odkazovaly na nově načtená entita</span><span class="sxs-lookup"><span data-stu-id="c240d-138">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="c240d-139">Dotazování souvisejících entit</span><span class="sxs-lookup"><span data-stu-id="c240d-139">Querying related entities</span></span>

<span data-ttu-id="c240d-140">Můžete také získat dotaz LINQ, který představuje obsah vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="c240d-140">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="c240d-141">To umožňuje provádět akce, jako je například spuštění agregačního operátoru nad souvisejícími entitami bez jejich načtení do paměti.</span><span class="sxs-lookup"><span data-stu-id="c240d-141">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="c240d-142">Můžete také filtrovat, které související entity jsou načteny do paměti.</span><span class="sxs-lookup"><span data-stu-id="c240d-142">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="c240d-143">opožděné načítání</span><span class="sxs-lookup"><span data-stu-id="c240d-143">Lazy loading</span></span>

<span data-ttu-id="c240d-144">Nejjednodušším způsobem, jak použít opožděné načítání, je nainstalovat balíček [Microsoft. EntityFrameworkCore. proxy](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) a povolit mu volání `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="c240d-144">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="c240d-145">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c240d-145">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```

<span data-ttu-id="c240d-146">Nebo při použití AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="c240d-146">Or when using AddDbContext:</span></span>

```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```

<span data-ttu-id="c240d-147">EF Core pak povolí opožděné načítání pro jakoukoliv navigační vlastnost, která může být přepsána – to znamená, že musí být `virtual` a na třídě, která může být děděna z.</span><span class="sxs-lookup"><span data-stu-id="c240d-147">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="c240d-148">Například v následujících entitách budou vlastnosti `Post.Blog` a `Blog.Posts` navigace opožděně načteny.</span><span class="sxs-lookup"><span data-stu-id="c240d-148">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>

```csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public virtual Blog Blog { get; set; }
}
```

### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="c240d-149">Opožděné načítání bez proxy serverů</span><span class="sxs-lookup"><span data-stu-id="c240d-149">Lazy loading without proxies</span></span>

<span data-ttu-id="c240d-150">Opožděné načítání proxy serverů funguje tak, že se služba `ILazyLoader` vloží do entity, jak je popsáno v tématu [konstruktory typu entity](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="c240d-150">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="c240d-151">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c240d-151">For example:</span></span>

```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```

<span data-ttu-id="c240d-152">Tato možnost nepožaduje dědění typů entit nebo vlastností navigace jako virtuálních a umožňuje, aby se instance entit vytvořily pomocí `new` opožděně načteny po připojení k kontextu.</span><span class="sxs-lookup"><span data-stu-id="c240d-152">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="c240d-153">Nicméně vyžaduje odkaz na službu `ILazyLoader`, která je definována v balíčku [Microsoft. EntityFrameworkCore. Abstracts](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) .</span><span class="sxs-lookup"><span data-stu-id="c240d-153">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="c240d-154">Tento balíček obsahuje minimální sadu typů, aby v závislosti na tom byl velmi malý vliv.</span><span class="sxs-lookup"><span data-stu-id="c240d-154">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="c240d-155">Pokud se však chcete zcela vyhnout v závislosti na jakémkoli EF Core balíčky v typech entit, je možné metodu `ILazyLoader.Load` vložit jako delegáta.</span><span class="sxs-lookup"><span data-stu-id="c240d-155">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="c240d-156">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c240d-156">For example:</span></span>

```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```

<span data-ttu-id="c240d-157">Výše uvedený kód používá metodu rozšíření `Load` k použití delegáta bitového čističe:</span><span class="sxs-lookup"><span data-stu-id="c240d-157">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>

```csharp
public static class PocoLoadingExtensions
{
    public static TRelated Load<TRelated>(
        this Action<object, string> loader,
        object entity,
        ref TRelated navigationField,
        [CallerMemberName] string navigationName = null)
        where TRelated : class
    {
        loader?.Invoke(entity, navigationName);

        return navigationField;
    }
}
```

> [!NOTE]  
> <span data-ttu-id="c240d-158">Parametr konstruktoru pro delegáta opožděného načítání musí být nazvaný "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="c240d-158">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="c240d-159">Konfigurace, která má použít jiný název než toto je plánována v budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="c240d-159">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="c240d-160">Související data a serializace</span><span class="sxs-lookup"><span data-stu-id="c240d-160">Related data and serialization</span></span>

<span data-ttu-id="c240d-161">Vzhledem k tomu, že EF Core bude automaticky opravovat navigační vlastnosti, můžete v grafu objektů končit cykly.</span><span class="sxs-lookup"><span data-stu-id="c240d-161">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="c240d-162">Například načtení blogu a jeho souvisejících příspěvků bude mít za následek blogový objekt, který odkazuje na kolekci příspěvků.</span><span class="sxs-lookup"><span data-stu-id="c240d-162">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="c240d-163">Každá z těchto příspěvků bude mít odkaz zpátky na blog.</span><span class="sxs-lookup"><span data-stu-id="c240d-163">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="c240d-164">Některé serializace rozhraní tyto cykly nepovolují.</span><span class="sxs-lookup"><span data-stu-id="c240d-164">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="c240d-165">Json.NET například vyvolá následující výjimku, pokud dojde k cyklu.</span><span class="sxs-lookup"><span data-stu-id="c240d-165">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="c240d-166">Newtonsoft. JSON. JsonSerializationException: byla zjištěna smyčka s odkazem na vlastnost ' blog ' s typem ' MyApplication. Models. blog '.</span><span class="sxs-lookup"><span data-stu-id="c240d-166">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="c240d-167">Pokud používáte ASP.NET Core, můžete Json.NET nakonfigurovat tak, aby ignorovala cykly, které najde v grafu objektů.</span><span class="sxs-lookup"><span data-stu-id="c240d-167">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="c240d-168">To se provádí v metodě `ConfigureServices(...)` v `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="c240d-168">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```

<span data-ttu-id="c240d-169">Další možností je vyjednat jednu z navigačních vlastností pomocí atributu `[JsonIgnore]`, který dává pokyn, aby při serializaci Json.NET, že se tato vlastnost navigace neprojde.</span><span class="sxs-lookup"><span data-stu-id="c240d-169">Another alternative is to decorate one of the navigation properties with the `[JsonIgnore]` attribute, which instructs Json.NET to not traverse that navigation property while serializing.</span></span>
