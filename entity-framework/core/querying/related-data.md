---
title: Načítají se související data – EF Core.
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 4bf9598f9b7e74c2835d3926215de9a7ef4e6f96
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921794"
---
# <a name="loading-related-data"></a><span data-ttu-id="4de70-102">Načítají se související data.</span><span class="sxs-lookup"><span data-stu-id="4de70-102">Loading Related Data</span></span>

<span data-ttu-id="4de70-103">Entity Framework Core umožňuje v modelu použít navigační vlastnosti pro načtení souvisejících entit.</span><span class="sxs-lookup"><span data-stu-id="4de70-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="4de70-104">Existují tři běžné vzory O/RM, které slouží k načtení souvisejících dat.</span><span class="sxs-lookup"><span data-stu-id="4de70-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="4de70-105">**Eager načítání** znamená, že související data jsou načtena z databáze jako součást počátečního dotazu.</span><span class="sxs-lookup"><span data-stu-id="4de70-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="4de70-106">**Explicitní načítání** znamená, že související data jsou explicitně načtena z databáze později.</span><span class="sxs-lookup"><span data-stu-id="4de70-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="4de70-107">**Opožděné načítání** znamená, že související data jsou transparentně načítána z databáze, pokud je k ní přistupovaná vlastnost navigace.</span><span class="sxs-lookup"><span data-stu-id="4de70-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="4de70-108">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="4de70-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="4de70-109">Eager načítání</span><span class="sxs-lookup"><span data-stu-id="4de70-109">Eager loading</span></span>

<span data-ttu-id="4de70-110">Pomocí `Include` metody můžete určit související data, která mají být součástí výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="4de70-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="4de70-111">V následujícím příkladu budou Blogy, které jsou vráceny ve výsledcích, mít svou `Posts` vlastnost naplněnou souvisejícími příspěvky.</span><span class="sxs-lookup"><span data-stu-id="4de70-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="4de70-112">Entity Framework Core bude automaticky opravovat navigační vlastnosti pro všechny další entity, které byly dříve načteny do instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="4de70-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="4de70-113">Takže i když nebudete explicitně vkládat data pro navigační vlastnost, může být tato vlastnost i nadále naplněna, pokud byly některé nebo všechny související entity dříve načteny.</span><span class="sxs-lookup"><span data-stu-id="4de70-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="4de70-114">V jednom dotazu můžete zahrnout související data z několika vztahů.</span><span class="sxs-lookup"><span data-stu-id="4de70-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="4de70-115">Zahrnutí více úrovní</span><span class="sxs-lookup"><span data-stu-id="4de70-115">Including multiple levels</span></span>

<span data-ttu-id="4de70-116">Můžete přejít k podrobnostem na základě vztahů, abyste zahrnuli více úrovní souvisejících `ThenInclude` dat pomocí metody.</span><span class="sxs-lookup"><span data-stu-id="4de70-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="4de70-117">Následující příklad načte všechny Blogy, jejich související příspěvky a autora každého příspěvku.</span><span class="sxs-lookup"><span data-stu-id="4de70-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="4de70-118">Aktuální verze sady Visual Studio nabízejí nesprávné možnosti doplňování kódu a mohou způsobit, že správné výrazy budou označeny chybami syntaxe při použití `ThenInclude` metody po navigační vlastnosti kolekce.</span><span class="sxs-lookup"><span data-stu-id="4de70-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="4de70-119">Toto je příznak chyby technologie IntelliSense sledované na adrese https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="4de70-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="4de70-120">Tyto chyby syntaxe spurious je bezpečné ignorovat, pokud je kód správný a je možné ho zkompilovat úspěšně.</span><span class="sxs-lookup"><span data-stu-id="4de70-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="4de70-121">Můžete zřetězit více volání, aby `ThenInclude` bylo možné pokračovat včetně dalších úrovní souvisejících dat.</span><span class="sxs-lookup"><span data-stu-id="4de70-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="4de70-122">To všechno můžete zkombinovat, chcete-li zahrnout související data z více úrovní a více kořenů ve stejném dotazu.</span><span class="sxs-lookup"><span data-stu-id="4de70-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="4de70-123">Možná budete chtít zahrnout několik souvisejících entit pro jednu z zahrnutých entit.</span><span class="sxs-lookup"><span data-stu-id="4de70-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="4de70-124">Například při `Blogs`dotazování zahrnete `Author` `Posts` a potom chcete zahrnout `Tags` i z `Posts`.</span><span class="sxs-lookup"><span data-stu-id="4de70-124">For example, when querying `Blogs`, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="4de70-125">Chcete-li to provést, je třeba zadat každou cestu zahrnutí počínaje kořenovým adresářem.</span><span class="sxs-lookup"><span data-stu-id="4de70-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="4de70-126">Například `Blog -> Posts -> Author` a `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="4de70-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="4de70-127">To neznamená, že budete mít redundantní spojení. ve většině případů bude EF konsolidovat spojení při generování SQL.</span><span class="sxs-lookup"><span data-stu-id="4de70-127">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="4de70-128">Zahrnout do odvozených typů</span><span class="sxs-lookup"><span data-stu-id="4de70-128">Include on derived types</span></span>

<span data-ttu-id="4de70-129">Můžete zahrnout související data z navigace definovaná pouze pro odvozený typ pomocí `Include` a. `ThenInclude`</span><span class="sxs-lookup"><span data-stu-id="4de70-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="4de70-130">S ohledem na následující model:</span><span class="sxs-lookup"><span data-stu-id="4de70-130">Given the following model:</span></span>

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

<span data-ttu-id="4de70-131">`School` Obsah navigace všech uživatelů, kteří jsou studenti, se dá eagerly načíst pomocí několika vzorů:</span><span class="sxs-lookup"><span data-stu-id="4de70-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="4de70-132">použití přetypování</span><span class="sxs-lookup"><span data-stu-id="4de70-132">using cast</span></span>
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="4de70-133">operátor `as` using</span><span class="sxs-lookup"><span data-stu-id="4de70-133">using `as` operator</span></span>
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="4de70-134">použití přetížení `Include` , které přijímá parametr typu`string`</span><span class="sxs-lookup"><span data-stu-id="4de70-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```csharp
  context.People.Include("School").ToList()
  ```

### <a name="ignored-includes"></a><span data-ttu-id="4de70-135">Ignorované zahrnutí</span><span class="sxs-lookup"><span data-stu-id="4de70-135">Ignored includes</span></span>

<span data-ttu-id="4de70-136">Pokud změníte dotaz tak, že již nadále nevrátí instance typu entity, na kterém dotaz začal, jsou operátory include ignorovány.</span><span class="sxs-lookup"><span data-stu-id="4de70-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="4de70-137">V následujícím příkladu jsou operátory include založeny na `Blog`, ale `Select` operátor se používá ke změně dotazu na vrácení anonymního typu.</span><span class="sxs-lookup"><span data-stu-id="4de70-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="4de70-138">V takovém případě nejsou operátory include nijak ovlivněny.</span><span class="sxs-lookup"><span data-stu-id="4de70-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="4de70-139">Ve výchozím nastavení EF Core zaznamená upozornění, když budou operátory include ignorovány.</span><span class="sxs-lookup"><span data-stu-id="4de70-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="4de70-140">Další informace o zobrazení výstupu protokolování najdete v tématu [protokolování](../miscellaneous/logging.md) .</span><span class="sxs-lookup"><span data-stu-id="4de70-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="4de70-141">Chování můžete změnit při ignorování operátoru include pro vyvolání nebo provedení akce.</span><span class="sxs-lookup"><span data-stu-id="4de70-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="4de70-142">To se provádí při nastavování možností pro váš kontext – obvykle v `DbContext.OnConfiguring`nebo v `Startup.cs` případě, že používáte ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4de70-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="4de70-143">explicitní načítání</span><span class="sxs-lookup"><span data-stu-id="4de70-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="4de70-144">Tato funkce byla představena v EF Core 1,1.</span><span class="sxs-lookup"><span data-stu-id="4de70-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="4de70-145">Můžete explicitně načíst navigační vlastnost prostřednictvím `DbContext.Entry(...)` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4de70-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="4de70-146">Můžete také explicitně načíst vlastnost navigace spuštěním samostatného dotazu, který vrací související entity.</span><span class="sxs-lookup"><span data-stu-id="4de70-146">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="4de70-147">Pokud je povoleno sledování změn, pak při načítání entity EF Core automaticky nastaví navigační vlastnosti nově načtených entity, aby odkazovaly na jakékoli již načtené entity, a nastavili navigační vlastnosti již načtených entit tak, aby odkazovaly na nově načtená entita</span><span class="sxs-lookup"><span data-stu-id="4de70-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="4de70-148">Dotazování souvisejících entit</span><span class="sxs-lookup"><span data-stu-id="4de70-148">Querying related entities</span></span>

<span data-ttu-id="4de70-149">Můžete také získat dotaz LINQ, který představuje obsah vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="4de70-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="4de70-150">To umožňuje provádět akce, jako je například spuštění agregačního operátoru nad souvisejícími entitami bez jejich načtení do paměti.</span><span class="sxs-lookup"><span data-stu-id="4de70-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="4de70-151">Můžete také filtrovat, které související entity jsou načteny do paměti.</span><span class="sxs-lookup"><span data-stu-id="4de70-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="4de70-152">opožděné načítání</span><span class="sxs-lookup"><span data-stu-id="4de70-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="4de70-153">Tato funkce byla představena v EF Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="4de70-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="4de70-154">Nejjednodušším způsobem, jak použít opožděné načítání, je nainstalovat balíček [Microsoft. EntityFrameworkCore. proxy](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) a povolit mu volání `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="4de70-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="4de70-155">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4de70-155">For example:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="4de70-156">Nebo při použití AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="4de70-156">Or when using AddDbContext:</span></span>
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
<span data-ttu-id="4de70-157">EF Core pak povolí opožděné načítání pro jakoukoliv navigační vlastnost, která může být přepsána – to znamená, že `virtual` musí být a na třídě, která může být děděna z.</span><span class="sxs-lookup"><span data-stu-id="4de70-157">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="4de70-158">Například v následujících entitách `Post.Blog` budou vlastnosti navigace a `Blog.Posts` aplikace opožděně načteny.</span><span class="sxs-lookup"><span data-stu-id="4de70-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
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
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="4de70-159">Opožděné načítání bez proxy serverů</span><span class="sxs-lookup"><span data-stu-id="4de70-159">Lazy loading without proxies</span></span>

<span data-ttu-id="4de70-160">Opožděné načítání proxy serverů funguje tak, že `ILazyLoader` službu vloží do entity, jak je popsáno v tématu [konstruktory typu entity](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="4de70-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="4de70-161">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4de70-161">For example:</span></span>
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
<span data-ttu-id="4de70-162">To nevyžaduje dědění typů entit nebo vlastností navigace jako virtuálních a umožňuje, aby se instance entit vytvořily s `new` opožděným načtením, a to po připojení k kontextu.</span><span class="sxs-lookup"><span data-stu-id="4de70-162">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="4de70-163">Nicméně vyžaduje odkaz na `ILazyLoader` službu, která je definována v balíčku [Microsoft. EntityFrameworkCore. Abstracts](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) .</span><span class="sxs-lookup"><span data-stu-id="4de70-163">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="4de70-164">Tento balíček obsahuje minimální sadu typů, aby v závislosti na tom byl velmi malý vliv.</span><span class="sxs-lookup"><span data-stu-id="4de70-164">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="4de70-165">Pokud se však chcete zcela vyhnout v závislosti na EF Core balíčky v typech entit, je možné `ILazyLoader.Load` metodu vložit jako delegáta.</span><span class="sxs-lookup"><span data-stu-id="4de70-165">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="4de70-166">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4de70-166">For example:</span></span>
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
<span data-ttu-id="4de70-167">Výše uvedený kód používá `Load` metodu rozšíření k použití delegáta bitového čističe:</span><span class="sxs-lookup"><span data-stu-id="4de70-167">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
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
> <span data-ttu-id="4de70-168">Parametr konstruktoru pro delegáta opožděného načítání musí být nazvaný "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="4de70-168">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="4de70-169">Konfigurace, která má použít jiný název než toto je plánována v budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="4de70-169">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="4de70-170">Související data a serializace</span><span class="sxs-lookup"><span data-stu-id="4de70-170">Related data and serialization</span></span>

<span data-ttu-id="4de70-171">Vzhledem k tomu, že EF Core bude automaticky opravovat navigační vlastnosti, můžete v grafu objektů končit cykly.</span><span class="sxs-lookup"><span data-stu-id="4de70-171">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="4de70-172">Například načtení blogu a jeho souvisejících příspěvků bude mít za následek blogový objekt, který odkazuje na kolekci příspěvků.</span><span class="sxs-lookup"><span data-stu-id="4de70-172">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="4de70-173">Každá z těchto příspěvků bude mít odkaz zpátky na blog.</span><span class="sxs-lookup"><span data-stu-id="4de70-173">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="4de70-174">Některé serializace rozhraní tyto cykly nepovolují.</span><span class="sxs-lookup"><span data-stu-id="4de70-174">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="4de70-175">Json.NET například vyvolá následující výjimku, pokud dojde k cyklu.</span><span class="sxs-lookup"><span data-stu-id="4de70-175">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="4de70-176">Newtonsoft. JSON. JsonSerializationException: Zjistila se smyčka s odkazem na vlastnost ' blog ' typu ' MyApplication. Models. blog '.</span><span class="sxs-lookup"><span data-stu-id="4de70-176">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="4de70-177">Pokud používáte ASP.NET Core, můžete Json.NET nakonfigurovat tak, aby ignorovala cykly, které najde v grafu objektů.</span><span class="sxs-lookup"><span data-stu-id="4de70-177">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="4de70-178">To se provádí v `ConfigureServices(...)` metodě v. `Startup.cs`</span><span class="sxs-lookup"><span data-stu-id="4de70-178">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

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

<span data-ttu-id="4de70-179">Další možností je vyjednat jednu z navigačních vlastností s `[JsonIgnore]` atributem, který instruuje JSON.NET, že při serializaci nebude procházet tuto vlastnost navigace.</span><span class="sxs-lookup"><span data-stu-id="4de70-179">Another alternative is to decorate one of the navigation properties with the `[JsonIgnore]` attribute, which instructs Json.NET to not traverse that navigation property while serializing.</span></span>
