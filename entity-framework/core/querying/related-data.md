---
title: Načítají se související Data – EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: dc8ef0b2f194fbedfaf58a9499719531a96f70da
ms.sourcegitcommit: 3d69ea20a7a0cca4d8e5d68bcfd3d1ee16950390
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/21/2018
ms.locfileid: "36299359"
---
# <a name="loading-related-data"></a><span data-ttu-id="d53d3-102">Načítání související Data</span><span class="sxs-lookup"><span data-stu-id="d53d3-102">Loading Related Data</span></span>

<span data-ttu-id="d53d3-103">Entity Framework Core umožňuje používat navigační vlastnosti v modelu se načíst související entity.</span><span class="sxs-lookup"><span data-stu-id="d53d3-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="d53d3-104">Existují tři obecné vzory O/RM používají k zatížení související data.</span><span class="sxs-lookup"><span data-stu-id="d53d3-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="d53d3-105">**Přes načítání** znamená, že související data načtená z databáze jako součást počáteční dotazu.</span><span class="sxs-lookup"><span data-stu-id="d53d3-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="d53d3-106">**Explicitní načítání** znamená, že související data se explicitně načíst z databáze později.</span><span class="sxs-lookup"><span data-stu-id="d53d3-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="d53d3-107">**Opožděného načítání** znamená, že související transparentně načtení dat z databáze při přístupu k navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d53d3-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="d53d3-108">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="d53d3-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="d53d3-109">přes načítání</span><span class="sxs-lookup"><span data-stu-id="d53d3-109">Eager loading</span></span>

<span data-ttu-id="d53d3-110">Můžete použít `Include` metoda k určení související data mají být zahrnuty do výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="d53d3-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="d53d3-111">V následujícím příkladu, bude mít blogy, které jsou vráceny ve výsledcích jejich `Posts` vlastnost naplněný související příspěvky.</span><span class="sxs-lookup"><span data-stu-id="d53d3-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="d53d3-112">Entity Framework Core bude automaticky opravu up navigačních vlastností pro ostatní entity, které byly dříve načteny do instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="d53d3-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="d53d3-113">Takže i v případě, že není výslovně zahrnout data pro navigační vlastnost, vlastnost pořád naplněný, pokud některé nebo všechny související entity byly dříve načteny.</span><span class="sxs-lookup"><span data-stu-id="d53d3-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="d53d3-114">Související data z více vztahů můžete zahrnout jeden dotaz.</span><span class="sxs-lookup"><span data-stu-id="d53d3-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="d53d3-115">Více úrovní</span><span class="sxs-lookup"><span data-stu-id="d53d3-115">Including multiple levels</span></span>

<span data-ttu-id="d53d3-116">Podrobnostem přispívají vztahů s zahrnují více úrovní související data pomocí `ThenInclude` metoda.</span><span class="sxs-lookup"><span data-stu-id="d53d3-116">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="d53d3-117">Následující příklad načte všechny blogy, jejich související příspěvky a Autor každé post.</span><span class="sxs-lookup"><span data-stu-id="d53d3-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="d53d3-118">Aktuální verze sady Visual Studio nabízí možnosti dokončení nesprávný kód a mohou způsobit správné výrazy označen příznakem chyby syntaxe při použití `ThenInclude` metoda po navigační vlastnost kolekce.</span><span class="sxs-lookup"><span data-stu-id="d53d3-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="d53d3-119">Toto je příznakem IntelliSense chyb sledovány v https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="d53d3-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="d53d3-120">Je bezpečné tyto chyby syntaxe nesprávné ignorovat, dokud je správný kód a mohou být zkompilovány úspěšně.</span><span class="sxs-lookup"><span data-stu-id="d53d3-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="d53d3-121">Můžete zřetězit více volání `ThenInclude` Chcete-li pokračovat, včetně další úrovně související data.</span><span class="sxs-lookup"><span data-stu-id="d53d3-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="d53d3-122">Můžete sloučit všechny tohoto zahrnout související data z více úrovní a více kořenových certifikačních autorit ve stejném dotazu.</span><span class="sxs-lookup"><span data-stu-id="d53d3-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="d53d3-123">Můžete zahrnout více entit v relaci pro jednu z entity, které bude zahrnut.</span><span class="sxs-lookup"><span data-stu-id="d53d3-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="d53d3-124">Například při dotazování `Blog`s, zahrnete `Posts` a chcete současně obsahovat `Author` a `Tags` z `Posts`.</span><span class="sxs-lookup"><span data-stu-id="d53d3-124">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="d53d3-125">K tomuto účelu, budete muset zadat jednotlivé obsahovat cestu spouštění v kořenovém adresáři.</span><span class="sxs-lookup"><span data-stu-id="d53d3-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="d53d3-126">Například `Blog -> Posts -> Author` a `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="d53d3-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="d53d3-127">To neznamená, že budete mít redundantní spojení, ve většině případů, které budou konsolidovat EF spojení při generování SQL.</span><span class="sxs-lookup"><span data-stu-id="d53d3-127">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="d53d3-128">Zahrnout na odvozené typy</span><span class="sxs-lookup"><span data-stu-id="d53d3-128">Include on derived types</span></span>

<span data-ttu-id="d53d3-129">Můžete zahrnout související data z navigací definovaný jenom pro odvozený typ pomocí `Include` a `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="d53d3-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="d53d3-130">Zadaný model následující:</span><span class="sxs-lookup"><span data-stu-id="d53d3-130">Given the following model:</span></span>

```Csharp
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

<span data-ttu-id="d53d3-131">Obsah `School` navigační všichni uživatelé, kteří jsou studenti, kteří mohou být načteny například, s počtem vzorků:</span><span class="sxs-lookup"><span data-stu-id="d53d3-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="d53d3-132">pomocí přetypování</span><span class="sxs-lookup"><span data-stu-id="d53d3-132">using cast</span></span>
  ```Csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="d53d3-133">pomocí `as` – operátor</span><span class="sxs-lookup"><span data-stu-id="d53d3-133">using `as` operator</span></span>
  ```Csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="d53d3-134">pomocí přetížení `Include` , která má parametr typu `string`</span><span class="sxs-lookup"><span data-stu-id="d53d3-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```Csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a><span data-ttu-id="d53d3-135">Ignorovat zahrnuje</span><span class="sxs-lookup"><span data-stu-id="d53d3-135">Ignored includes</span></span>

<span data-ttu-id="d53d3-136">Pokud změníte dotaz tak, že už vrátí instance typu entity, které začne dotaz s, se ignorují operátory zahrnout.</span><span class="sxs-lookup"><span data-stu-id="d53d3-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="d53d3-137">V následujícím příkladu jsou na základě operátory zahrnout `Blog`, ale pak `Select` operátor se používá ke změně dotaz vrátit instanci anonymního typu.</span><span class="sxs-lookup"><span data-stu-id="d53d3-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="d53d3-138">V takovém případě operátory zahrnout nemají žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="d53d3-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="d53d3-139">Ve výchozím nastavení, bude EF základní zaprotokolovat upozornění při zahrnují operátory jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="d53d3-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="d53d3-140">V tématu [protokolování](../miscellaneous/logging.md) Další informace o zobrazení výstupu protokolování.</span><span class="sxs-lookup"><span data-stu-id="d53d3-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="d53d3-141">Chování lze změnit, pokud zahrnout operátor je ignorován throw nebo nic nestane.</span><span class="sxs-lookup"><span data-stu-id="d53d3-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="d53d3-142">To se provádí při nastavování možnosti pro váš kontext – obvykle ve `DbContext.OnConfiguring`, nebo v `Startup.cs` Pokud používáte ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d53d3-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="d53d3-143">explicitní načítání</span><span class="sxs-lookup"><span data-stu-id="d53d3-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="d53d3-144">Tato funkce byla zavedená v EF základní 1.1.</span><span class="sxs-lookup"><span data-stu-id="d53d3-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="d53d3-145">Můžete explicitně načíst navigační vlastnost prostřednictvím `DbContext.Entry(...)` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d53d3-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="d53d3-146">Můžete také explicitně načíst navigační vlastnost spuštěním další dotaz, který vrací související entity.</span><span class="sxs-lookup"><span data-stu-id="d53d3-146">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="d53d3-147">Pokud je povoleno sledování změn, pak při načítání entitu, EF základní bude automaticky nastavit navigační vlastnosti nově načíst entitiy odkazovat na všechny entity již načten a vlastnosti navigace entit již načten k odkazování na nově načíst entity.</span><span class="sxs-lookup"><span data-stu-id="d53d3-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="d53d3-148">Dotazování entit v relaci</span><span class="sxs-lookup"><span data-stu-id="d53d3-148">Querying related entities</span></span>

<span data-ttu-id="d53d3-149">Můžete také získat LINQ dotazu, který reprezentuje obsah navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d53d3-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="d53d3-150">To umožňuje provádět akce, jako je například spuštění agregační operátor přes související entity bez jejich načtení do paměti.</span><span class="sxs-lookup"><span data-stu-id="d53d3-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="d53d3-151">Můžete také filtrovat, které entit v relaci jsou načtena do paměti.</span><span class="sxs-lookup"><span data-stu-id="d53d3-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="d53d3-152">opožděného načítání</span><span class="sxs-lookup"><span data-stu-id="d53d3-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="d53d3-153">Tato funkce byla zavedená v EF základní 2.1.</span><span class="sxs-lookup"><span data-stu-id="d53d3-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="d53d3-154">Nejjednodušší způsob, jak používat opožděného načítání, je instalace [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) balíček a jeho povolení pomocí volání `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="d53d3-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="d53d3-155">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d53d3-155">For example:</span></span>
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="d53d3-156">Nebo pokud používáte AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="d53d3-156">Or when using AddDbContext:</span></span>
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
<span data-ttu-id="d53d3-157">Základní EF potom umožní opožděného načítání pro vlastnost navigace, která je možné přepsat – a který je, musí být `virtual` a na třídu, která je možné zdědit z.</span><span class="sxs-lookup"><span data-stu-id="d53d3-157">EF Core will then enable lazy-loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="d53d3-158">Například v následujících entit `Post.Blog` a `Blog.Posts` navigační vlastnosti bude opožděné načíst.</span><span class="sxs-lookup"><span data-stu-id="d53d3-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
```Csharp
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
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="d53d3-159">Lazy načítání bez proxy</span><span class="sxs-lookup"><span data-stu-id="d53d3-159">Lazy-loading without proxies</span></span>

<span data-ttu-id="d53d3-160">Lazy načítání proxy fungovat vložením `ILazyLoader` služby do entity, jak je popsáno v [konstruktory typu Entity](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="d53d3-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="d53d3-161">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d53d3-161">For example:</span></span>
```Csharp
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
<span data-ttu-id="d53d3-162">To nevyžaduje typy entit byla zděděna z a navigačních vlastností pro virtuální a umožňuje instancí entit, které jsou vytvořené pomocí `new` opožděné načtení jednou připojené k kontextu.</span><span class="sxs-lookup"><span data-stu-id="d53d3-162">This doesn't require entity types to be inherited from or navigation properties to be virtual and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="d53d3-163">To ale vyžaduje odkaz na `ILazyLoader` služba, která je definována v [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="d53d3-163">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="d53d3-164">Tento balíček obsahuje minimální sadu typů tak, aby bylo velmi malý vliv v v závislosti na jeho.</span><span class="sxs-lookup"><span data-stu-id="d53d3-164">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="d53d3-165">Abyste se vyhnuli zcela v závislosti na balíčcích EF základní v typy entit, je však možné vložit `ILazyLoader.Load` metoda jako delegáta.</span><span class="sxs-lookup"><span data-stu-id="d53d3-165">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="d53d3-166">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d53d3-166">For example:</span></span>
```Csharp
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
<span data-ttu-id="d53d3-167">Kód výše používá `Load` metoda rozšíření, aby pomocí delegát trochu čisticí:</span><span class="sxs-lookup"><span data-stu-id="d53d3-167">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
```Csharp
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
> <span data-ttu-id="d53d3-168">Parametr konstruktoru pro delegáta opožděného načítání musí být voláno "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="d53d3-168">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="d53d3-169">Konfigurace použijte jiný název, který to je plánovaná pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="d53d3-169">Configuration to use a different name this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="d53d3-170">Související data a serializace</span><span class="sxs-lookup"><span data-stu-id="d53d3-170">Related data and serialization</span></span>

<span data-ttu-id="d53d3-171">Protože základní EF bude automaticky opravu up navigační vlastnosti, můžete skončili s cykly v grafu objektu.</span><span class="sxs-lookup"><span data-stu-id="d53d3-171">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="d53d3-172">Například načítání blog a nesouvisí způsobí příspěvcích na blogu objekt, který odkazuje na kolekci zpráv.</span><span class="sxs-lookup"><span data-stu-id="d53d3-172">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="d53d3-173">Každý z těchto příspěvcích bude mít odkaz na blogu.</span><span class="sxs-lookup"><span data-stu-id="d53d3-173">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="d53d3-174">Některé architektury serializace neumožňují takové cykly.</span><span class="sxs-lookup"><span data-stu-id="d53d3-174">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="d53d3-175">Například Json.NET vyvolá následující výjimka, pokud se zjistil cyklický odkaz.</span><span class="sxs-lookup"><span data-stu-id="d53d3-175">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="d53d3-176">Newtonsoft.Json.JsonSerializationException: Vlastní odkazující na smyčky zjištěna vlastnost 'Blog' typu 'MyApplication.Models.Blog'.</span><span class="sxs-lookup"><span data-stu-id="d53d3-176">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="d53d3-177">Pokud používáte ASP.NET Core, můžete nakonfigurovat Json.NET ignorovat cykly, které najde v grafu objektů.</span><span class="sxs-lookup"><span data-stu-id="d53d3-177">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="d53d3-178">To se provádí v `ConfigureServices(...)` metoda v `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="d53d3-178">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

``` csharp
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
