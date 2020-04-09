---
title: Načítání souvisejících dat – jádro EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 915aaa41beb495a046f2d6260e9c3b174d5f3031
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417675"
---
# <a name="loading-related-data"></a><span data-ttu-id="12626-102">Načítání souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="12626-102">Loading Related Data</span></span>

<span data-ttu-id="12626-103">Entity Framework Core umožňuje použít navigační vlastnosti v modelu k načtení souvisejících entit.</span><span class="sxs-lookup"><span data-stu-id="12626-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="12626-104">K načtení souvisejících dat se používají tři běžné vzory O/RM.</span><span class="sxs-lookup"><span data-stu-id="12626-104">There are three common O/RM patterns used to load related data.</span></span>

* <span data-ttu-id="12626-105">**Dychtivé načítání** znamená, že související data se načtou z databáze jako součást počátečního dotazu.</span><span class="sxs-lookup"><span data-stu-id="12626-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="12626-106">**Explicitní načítání** znamená, že související data jsou explicitně načtena z databáze později.</span><span class="sxs-lookup"><span data-stu-id="12626-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="12626-107">**Opožděné načítání** znamená, že související data jsou transparentně načtena z databáze při přístupu k vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="12626-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="12626-108">Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="12626-108">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="12626-109">Dychtivé načítání</span><span class="sxs-lookup"><span data-stu-id="12626-109">Eager loading</span></span>

<span data-ttu-id="12626-110">Tuto metodu `Include` můžete použít k určení souvisejících dat, která mají být zahrnuta do výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="12626-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="12626-111">V následujícím příkladu blogy, které jsou vráceny `Posts` ve výsledcích bude mít jejich vlastnost naplněna související příspěvky.</span><span class="sxs-lookup"><span data-stu-id="12626-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="12626-112">Jádro frameworku entit automaticky opraví navigační vlastnosti na všechny ostatní entity, které byly dříve načteny do instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="12626-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="12626-113">Takže i v případě, že explicitně nezahrnete data pro navigační vlastnost, vlastnost může být stále naplněna, pokud byly dříve načteny některé nebo všechny související entity.</span><span class="sxs-lookup"><span data-stu-id="12626-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

<span data-ttu-id="12626-114">Do jednoho dotazu můžete zahrnout související data z více relací.</span><span class="sxs-lookup"><span data-stu-id="12626-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="12626-115">Včetně více úrovní</span><span class="sxs-lookup"><span data-stu-id="12626-115">Including multiple levels</span></span>

<span data-ttu-id="12626-116">Pomocí `ThenInclude` metody můžete procházet relacemi a zahrnout více úrovní souvisejících dat.</span><span class="sxs-lookup"><span data-stu-id="12626-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="12626-117">Následující příklad načte všechny blogy, jejich související příspěvky a autora každého příspěvku.</span><span class="sxs-lookup"><span data-stu-id="12626-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

<span data-ttu-id="12626-118">Můžete zřetězit `ThenInclude` více volání pokračovat, včetně další úrovně souvisejících dat.</span><span class="sxs-lookup"><span data-stu-id="12626-118">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="12626-119">To vše můžete zkombinovat tak, aby zahrnovala související data z více úrovní a více kořenů ve stejném dotazu.</span><span class="sxs-lookup"><span data-stu-id="12626-119">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="12626-120">Můžete chtít zahrnout více souvisejících entit pro jednu z entit, které jsou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="12626-120">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="12626-121">Například `Blogs`při dotazování , `Posts` zahrnout a potom `Author` chcete `Tags` zahrnout `Posts`jak a a .</span><span class="sxs-lookup"><span data-stu-id="12626-121">For example, when querying `Blogs`, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="12626-122">Chcete-li to provést, je třeba zadat každou cestu zahrnutí začínající v kořenovém adresáři.</span><span class="sxs-lookup"><span data-stu-id="12626-122">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="12626-123">Například `Blog -> Posts -> Author` a `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="12626-123">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="12626-124">To neznamená, že dostanete nadbytečné spojení; ve většině případů EF bude konsolidovat spojení při generování SQL.</span><span class="sxs-lookup"><span data-stu-id="12626-124">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

> [!CAUTION]
> <span data-ttu-id="12626-125">Od verze 3.0.0, každý `Include` způsobí další JOIN přidat do dotazů SQL vytvářených relační zprostředkovatelé, zatímco předchozí verze generované další dotazy SQL.</span><span class="sxs-lookup"><span data-stu-id="12626-125">Since version 3.0.0, each `Include` will cause an additional JOIN to be added to SQL queries produced by relational providers, whereas previous versions generated additional SQL queries.</span></span> <span data-ttu-id="12626-126">To může výrazně změnit výkon dotazů, v dobrém i ve zlém.</span><span class="sxs-lookup"><span data-stu-id="12626-126">This can significantly change the performance of your queries, for better or worse.</span></span> <span data-ttu-id="12626-127">Zejména linq dotazy s mimořádně vysokým počtem `Include` operátorů může být nutné rozdělit do více samostatných linq dotazy, aby se zabránilo problém s kartézním rozpadem.</span><span class="sxs-lookup"><span data-stu-id="12626-127">In particular, LINQ queries with an exceedingly high number of `Include` operators may need to be broken down into multiple separate LINQ queries in order to avoid the cartesian explosion problem.</span></span>

### <a name="include-on-derived-types"></a><span data-ttu-id="12626-128">Zahrnout na odvozené typy</span><span class="sxs-lookup"><span data-stu-id="12626-128">Include on derived types</span></span>

<span data-ttu-id="12626-129">Související data z navigace můžete zahrnout pouze na `Include` odvozeném typu pomocí a `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="12626-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span>

<span data-ttu-id="12626-130">Vzhledem k následujícímu modelu:</span><span class="sxs-lookup"><span data-stu-id="12626-130">Given the following model:</span></span>

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

<span data-ttu-id="12626-131">Obsah `School` navigace všech lidí, kteří jsou studenty, lze dychtivě načíst pomocí několika vzorů:</span><span class="sxs-lookup"><span data-stu-id="12626-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

* <span data-ttu-id="12626-132">použití přetypážení</span><span class="sxs-lookup"><span data-stu-id="12626-132">using cast</span></span>

  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

* <span data-ttu-id="12626-133">pomocí `as` operátoru</span><span class="sxs-lookup"><span data-stu-id="12626-133">using `as` operator</span></span>

  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

* <span data-ttu-id="12626-134">použití `Include` přetížení, které má parametr typu`string`</span><span class="sxs-lookup"><span data-stu-id="12626-134">using overload of `Include` that takes parameter of type `string`</span></span>

  ```csharp
  context.People.Include("School").ToList()
  ```

## <a name="explicit-loading"></a><span data-ttu-id="12626-135">Explicitní načítání</span><span class="sxs-lookup"><span data-stu-id="12626-135">Explicit loading</span></span>

<span data-ttu-id="12626-136">Můžete explicitně načíst navigační `DbContext.Entry(...)` vlastnost prostřednictvím rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="12626-136">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="12626-137">Můžete také explicitně načíst vlastnost navigace spuštěním samostatného dotazu, který vrací související entity.</span><span class="sxs-lookup"><span data-stu-id="12626-137">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="12626-138">Pokud je povoleno sledování změn, pak při načítání entity EF Core automaticky nastaví navigační vlastnosti nově načtené entity tak, aby odkazovaly na všechny entity, které již byly načteny, a nastaví navigační vlastnosti již načtených entit tak, aby odkazovaly na nově načtenou entitu.</span><span class="sxs-lookup"><span data-stu-id="12626-138">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="12626-139">Dotazování souvisejících entit</span><span class="sxs-lookup"><span data-stu-id="12626-139">Querying related entities</span></span>

<span data-ttu-id="12626-140">Můžete také získat dotaz LINQ, který představuje obsah navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="12626-140">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="12626-141">To umožňuje například spuštění agregačního operátoru přes související entity bez jejich načtení do paměti.</span><span class="sxs-lookup"><span data-stu-id="12626-141">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="12626-142">Můžete také filtrovat, které související entity jsou načteny do paměti.</span><span class="sxs-lookup"><span data-stu-id="12626-142">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="12626-143">Opožděné načtení</span><span class="sxs-lookup"><span data-stu-id="12626-143">Lazy loading</span></span>

<span data-ttu-id="12626-144">Nejjednodušší způsob, jak používat opožděné načítání je instalací balíčku [Microsoft.EntityFrameworkCore.Proxy](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) a `UseLazyLoadingProxies`povolení s voláním .</span><span class="sxs-lookup"><span data-stu-id="12626-144">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="12626-145">Příklad:</span><span class="sxs-lookup"><span data-stu-id="12626-145">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```

<span data-ttu-id="12626-146">Nebo při použití AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="12626-146">Or when using AddDbContext:</span></span>

```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```

<span data-ttu-id="12626-147">EF Core pak povolí opožděné načítání pro všechny navigační vlastnosti, `virtual` které mohou být přepsány -- to znamená, že musí být a na třídu, která může být zděděna z.</span><span class="sxs-lookup"><span data-stu-id="12626-147">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="12626-148">Například v následujících `Post.Blog` entitách `Blog.Posts` a navigační vlastnosti budou opožděně načteny.</span><span class="sxs-lookup"><span data-stu-id="12626-148">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>

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

### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="12626-149">Opožděné načítání bez proxy serverů</span><span class="sxs-lookup"><span data-stu-id="12626-149">Lazy loading without proxies</span></span>

<span data-ttu-id="12626-150">Opožděné načítání proxy servery práce `ILazyLoader` vstřikováním služby do entity, jak je popsáno v [konstruktory typu entity](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="12626-150">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="12626-151">Příklad:</span><span class="sxs-lookup"><span data-stu-id="12626-151">For example:</span></span>

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

<span data-ttu-id="12626-152">To nevyžaduje, aby typy entit byly zděděny nebo navigační vlastnosti `new` byly virtuální, a umožňuje instance entit vytvořené s opožděným načtením, jakmile jsou připojeny k kontextu.</span><span class="sxs-lookup"><span data-stu-id="12626-152">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="12626-153">Vyžaduje však odkaz na `ILazyLoader` službu, která je definována v balíčku [Microsoft.EntityFrameworkCore.Abstractions.](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/)</span><span class="sxs-lookup"><span data-stu-id="12626-153">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="12626-154">Tento balíček obsahuje minimální sadu typů tak, aby v závislosti na něm byl velmi malý dopad.</span><span class="sxs-lookup"><span data-stu-id="12626-154">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="12626-155">Chcete-li se však zcela vyhnout v závislosti na všech balíčcích EF Core v typech entit, je možné vložit metodu `ILazyLoader.Load` jako delegát.</span><span class="sxs-lookup"><span data-stu-id="12626-155">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="12626-156">Příklad:</span><span class="sxs-lookup"><span data-stu-id="12626-156">For example:</span></span>

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

<span data-ttu-id="12626-157">Výše uvedený kód `Load` používá metodu rozšíření, aby se pomocí delegáta trochu čistší:</span><span class="sxs-lookup"><span data-stu-id="12626-157">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>

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
> <span data-ttu-id="12626-158">Parametr konstruktoru pro delegáta s opožděným načítáním se musí nazývat "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="12626-158">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="12626-159">Konfigurace pro použití jiného názvu, než je plánováno pro budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="12626-159">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="12626-160">Související data a serializace</span><span class="sxs-lookup"><span data-stu-id="12626-160">Related data and serialization</span></span>

<span data-ttu-id="12626-161">Vzhledem k tomu, že EF Core automaticky opraví navigační vlastnosti, můžete skončit s cykly v grafu objektu.</span><span class="sxs-lookup"><span data-stu-id="12626-161">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="12626-162">Například načtení blogu a jeho souvisejících příspěvků bude mít za následek objekt blogu, který odkazuje na kolekci příspěvků.</span><span class="sxs-lookup"><span data-stu-id="12626-162">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="12626-163">Každý z těchto příspěvků bude mít odkaz zpět na blog.</span><span class="sxs-lookup"><span data-stu-id="12626-163">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="12626-164">Některé architektury serializace neumožňují takové cykly.</span><span class="sxs-lookup"><span data-stu-id="12626-164">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="12626-165">Například Json.NET vyvolá následující výjimku, pokud dojde k cyklu.</span><span class="sxs-lookup"><span data-stu-id="12626-165">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="12626-166">Newtonsoft.Json.JsonSerializationException: Self referencing smyčky zjištěna pro vlastnost 'Blog' s typem 'MyApplication.Models.Blog'.</span><span class="sxs-lookup"><span data-stu-id="12626-166">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="12626-167">Pokud používáte ASP.NET Core, můžete nakonfigurovat Json.NET ignorovat cykly, které najde v grafu objektu.</span><span class="sxs-lookup"><span data-stu-id="12626-167">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="12626-168">To se provádí `ConfigureServices(...)` v `Startup.cs`metodě v .</span><span class="sxs-lookup"><span data-stu-id="12626-168">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

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

<span data-ttu-id="12626-169">Další alternativou je ozdobit jednu `[JsonIgnore]` z navigačních vlastností atributem, který Json.NET neprocházet tuto vlastnost navigace při serializaci.</span><span class="sxs-lookup"><span data-stu-id="12626-169">Another alternative is to decorate one of the navigation properties with the `[JsonIgnore]` attribute, which instructs Json.NET to not traverse that navigation property while serializing.</span></span>
