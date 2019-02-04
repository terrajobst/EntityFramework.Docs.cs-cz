---
title: Načítají se související Data – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 4e042acb805c743ee794f4e61105b8d2136973b1
ms.sourcegitcommit: 159c2e9afed7745e7512730ffffaf154bcf2ff4a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668723"
---
# <a name="loading-related-data"></a><span data-ttu-id="7ff2b-102">Načítání souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="7ff2b-102">Loading Related Data</span></span>

<span data-ttu-id="7ff2b-103">Entity Framework Core umožňuje načtení souvisejících entit pomocí vlastnosti navigace v modelu.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="7ff2b-104">Existují tři běžné modely O/RM umožňují načíst související data.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="7ff2b-105">**Předběžné načítání** znamená, že je související data načtená z databáze jako součást počátečního dotazu.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="7ff2b-106">**Explicitní načtení** znamená, že související explicitní načtení dat z databáze později.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="7ff2b-107">**Opožděné načtení** znamená, že související transparentně načtení dat z databáze při přístupu k navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="7ff2b-108">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="7ff2b-109">Předběžné načítání</span><span class="sxs-lookup"><span data-stu-id="7ff2b-109">Eager loading</span></span>

<span data-ttu-id="7ff2b-110">Můžete použít `Include` metodu pro určení související data mají být zahrnuty ve výsledcích dotazu.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="7ff2b-111">V následujícím příkladu, blogy, které jsou vráceny ve výsledcích mají jejich `Posts` vlastnost naplní souvisejících příspěvků.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="7ff2b-112">Entity Framework Core se automaticky opravit navigační vlastnosti s jinými entitami, které byly dříve načtena do instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="7ff2b-113">Takže i v případě, že není explicitně zahrnout data pro navigační vlastnost, vlastnost pořád naplněný, pokud některé nebo všechny související entity byly dříve načteny.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="7ff2b-114">Můžou obsahovat související data z více vztahů v jediném dotazu.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="7ff2b-115">Více úrovní</span><span class="sxs-lookup"><span data-stu-id="7ff2b-115">Including multiple levels</span></span>

<span data-ttu-id="7ff2b-116">Můžete procházet hierarchii prostřednictvím relace zahrnout související data s využitím více úrovní `ThenInclude` metody.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="7ff2b-117">Následující příklad načte všechny blogy, jejich souvisejících příspěvků a Autor každý příspěvek.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="7ff2b-118">Aktuální verze sady Visual Studio nabízí možnosti dokončení nesprávný kód a může způsobit správné výrazy označen příznakem chyby syntaxe při použití `ThenInclude` za navigační vlastnost kolekce.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="7ff2b-119">Toto je příznakem chyby IntelliSense sledovány v https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="7ff2b-120">Je bezpečné ignorovat tyto chyby syntaxe detekováno falešné jako kód je správný a může být zkompilován úspěšně.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="7ff2b-121">Můžete zřetězit více volání `ThenInclude` pokračovat, včetně dalších úrovní související data.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="7ff2b-122">Můžete kombinovat, abyste mohli zahrnout související data z více úrovní a více kořenových adresářů ve stejném dotazu.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="7ff2b-123">Můžete chtít zahrnout více souvisejících entit pro jeden z entity, které je zahrnuto.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="7ff2b-124">Třeba při dotazování `Blog`s, zahrnete `Posts` a poté chcete provést oba `Author` a `Tags` z `Posts`.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-124">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="7ff2b-125">Chcete-li to provést, musíte zadat jednotlivé obsahovat počínaje kořenovou cestu.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="7ff2b-126">Například `Blog -> Posts -> Author` a `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="7ff2b-127">To neznamená, že se zobrazí redundantní spojení; ve většině případů bude EF konsolidovat spojení při generování SQL.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-127">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="7ff2b-128">Umístit na odvozené typy</span><span class="sxs-lookup"><span data-stu-id="7ff2b-128">Include on derived types</span></span>

<span data-ttu-id="7ff2b-129">Můžete zahrnout související data z navigaci definovat jen pro odvozený typ pomocí `Include` a `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="7ff2b-130">Daný následující model:</span><span class="sxs-lookup"><span data-stu-id="7ff2b-130">Given the following model:</span></span>

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

<span data-ttu-id="7ff2b-131">Obsah `School` navigaci ve všech uživatelů, kteří studenty lze například načíst pomocí několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="7ff2b-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="7ff2b-132">pomocí přetypování</span><span class="sxs-lookup"><span data-stu-id="7ff2b-132">using cast</span></span>
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="7ff2b-133">pomocí `as` – operátor</span><span class="sxs-lookup"><span data-stu-id="7ff2b-133">using `as` operator</span></span>
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="7ff2b-134">pomocí přetížení `Include` , která přebírá parametr typu `string`</span><span class="sxs-lookup"><span data-stu-id="7ff2b-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a><span data-ttu-id="7ff2b-135">Ignorovat zahrnuje</span><span class="sxs-lookup"><span data-stu-id="7ff2b-135">Ignored includes</span></span>

<span data-ttu-id="7ff2b-136">Pokud změníte dotaz tak, aby by již nevracelo instancí typu entity, které se začnou dotazu, operátory zahrnout ignorovány.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="7ff2b-137">V následujícím příkladu je na základě operátory zahrnout `Blog`, ale pak `Select` operátor se používá ke změně dotaz, který vrací anonymního typu.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="7ff2b-138">V takovém případě operátory zahrnout nemají žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="7ff2b-139">Ve výchozím nastavení, se budou protokolovat EF Core upozornění při operátory jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="7ff2b-140">Zobrazit [protokolování](../miscellaneous/logging.md) pro další informace o prohlížení uložit výstup protokolování.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="7ff2b-141">Můžete změnit chování při zahrnutí operátor ignorován provést operaci throw nebo Neprovádět žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="7ff2b-142">To se provádí při nastavování možnosti pro váš kontext – obvykle v `DbContext.OnConfiguring`, nebo v `Startup.cs` Pokud používáte ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="7ff2b-143">explicitní načtení</span><span class="sxs-lookup"><span data-stu-id="7ff2b-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="7ff2b-144">Tato funkce byla zavedená v EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="7ff2b-145">Můžete explicitně načíst navigační vlastnost via `DbContext.Entry(...)` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="7ff2b-146">Navigační vlastnost můžete také explicitně načíst spuštěním samostatného dotaz, který vrátí související entity.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-146">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="7ff2b-147">Pokud je povoleno sledování změn, pak při načítání entity, EF Core automaticky nastavit vlastnosti navigace entitiy nově načíst k odkazování na všechny entity již načtena a nastavit vlastnosti navigace entit již načtena jako reference nově načtení entity.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="7ff2b-148">Dotazování na související entity</span><span class="sxs-lookup"><span data-stu-id="7ff2b-148">Querying related entities</span></span>

<span data-ttu-id="7ff2b-149">Můžete také získat dotaz LINQ, který představuje obsah navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="7ff2b-150">To umožňuje provádění akcí, například spuštění agregační operátor souvisejícími entitami bez jejich načtení do paměti.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="7ff2b-151">Můžete také filtrovat, které související entity jsou načtena do paměti.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="7ff2b-152">Opožděné načtení</span><span class="sxs-lookup"><span data-stu-id="7ff2b-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="7ff2b-153">Tato funkce byla zavedená v EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="7ff2b-154">Po instalaci je nejjednodušší způsob, jak pomocí opožděné načtení [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) balíček a jeho povolení voláním `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="7ff2b-155">Příklad:</span><span class="sxs-lookup"><span data-stu-id="7ff2b-155">For example:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="7ff2b-156">Nebo při použití AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="7ff2b-156">Or when using AddDbContext:</span></span>
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
<span data-ttu-id="7ff2b-157">EF Core se povolit opožděné načtení pro navigační vlastnost, která může být přepsána – to znamená, musí být `virtual` a na třídu, která je možné zdědit z.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-157">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="7ff2b-158">Například v následující entity `Post.Blog` a `Blog.Posts` navigační vlastnosti budou opožděné načtení.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
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
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="7ff2b-159">Opožděné načtení bez proxy servery</span><span class="sxs-lookup"><span data-stu-id="7ff2b-159">Lazy loading without proxies</span></span>

<span data-ttu-id="7ff2b-160">Opožděné načtení proxy servery fungují tak, že vkládá `ILazyLoader` služby do entity, jak je popsáno v [konstruktory typů entit](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="7ff2b-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="7ff2b-161">Příklad:</span><span class="sxs-lookup"><span data-stu-id="7ff2b-161">For example:</span></span>
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
<span data-ttu-id="7ff2b-162">To nevyžaduje, aby typy entit, aby se dědila z a navigačních vlastností pro virtuální a umožní instancí entit, které jsou vytvořené pomocí `new` opožděné načtení jednou připojené do kontextu.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-162">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="7ff2b-163">To ale vyžaduje odkaz na `ILazyLoader` služba, která je definována v [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-163">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="7ff2b-164">Tento balíček obsahuje minimální sadu typů tak, aby se velmi malý vliv v ji.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-164">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="7ff2b-165">Pokud chcete úplně vyhnout v závislosti na všechny balíčky EF Core v typech entit, je však možné vložit `ILazyLoader.Load` metoda jako delegát.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-165">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="7ff2b-166">Příklad:</span><span class="sxs-lookup"><span data-stu-id="7ff2b-166">For example:</span></span>
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
<span data-ttu-id="7ff2b-167">Kód výše používá `Load` metodu rozšíření k trochu čisticí využitím delegáta:</span><span class="sxs-lookup"><span data-stu-id="7ff2b-167">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
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
> <span data-ttu-id="7ff2b-168">Parametr konstruktoru pro opožděné načtení delegáta se musí volat "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="7ff2b-168">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="7ff2b-169">Konfigurace, aby používala jiný název než to je plánovaná pro budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-169">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="7ff2b-170">Serializace a souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="7ff2b-170">Related data and serialization</span></span>

<span data-ttu-id="7ff2b-171">Protože EF Core se automaticky opravit navigačních vlastností můžete skončit s cykly v grafu objektu.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-171">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="7ff2b-172">Například načítání blogu a jeho souvisejících příspěvků výsledkem bude objekt blogu, který odkazuje na kolekci příspěvků.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-172">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="7ff2b-173">Každá z těchto míst bude mít odkaz na blogu.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-173">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="7ff2b-174">Některé architektury serializace neumožňují takové cykly.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-174">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="7ff2b-175">Například Json.NET vyvolá následující výjimku, pokud dochází k zacyklení.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-175">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="7ff2b-176">Newtonsoft.Json.JsonSerializationException: Vlastní odkazující na zjištěna pro vlastnost "Blogu" typu "MyApplication.Models.Blog" smyčka.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-176">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="7ff2b-177">Pokud používáte ASP.NET Core, můžete nakonfigurovat Json.NET ignorovat cykly, které najde v grafu objektů.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-177">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="7ff2b-178">To se provádí v `ConfigureServices(...)` metoda ve `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="7ff2b-178">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

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
