---
title: Načítají se související data – EF Core.
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 4e4ba21cd099daab4db8a8f358800fde26980c14
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813577"
---
# <a name="loading-related-data"></a>Načítání souvisejících dat

Entity Framework Core umožňuje v modelu použít navigační vlastnosti pro načtení souvisejících entit. Existují tři běžné vzory O/RM, které slouží k načtení souvisejících dat.
* **Eager načítání** znamená, že související data jsou načtena z databáze jako součást počátečního dotazu.
* **Explicitní načítání** znamená, že související data jsou explicitně načtena z databáze později.
* **Opožděné načítání** znamená, že související data jsou transparentně načítána z databáze, pokud je k ní přistupovaná vlastnost navigace.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.

## <a name="eager-loading"></a>Eager načítání

Pomocí `Include` metody můžete určit související data, která mají být součástí výsledků dotazu. V následujícím příkladu budou Blogy, které jsou vráceny ve výsledcích, mít svou `Posts` vlastnost naplněnou souvisejícími příspěvky.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core bude automaticky opravovat navigační vlastnosti pro všechny další entity, které byly dříve načteny do instance kontextu. Takže i když nebudete explicitně vkládat data pro navigační vlastnost, může být tato vlastnost i nadále naplněna, pokud byly některé nebo všechny související entity dříve načteny.

V jednom dotazu můžete zahrnout související data z několika vztahů.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Zahrnutí více úrovní

Můžete přejít k podrobnostem na základě vztahů, abyste zahrnuli více úrovní souvisejících `ThenInclude` dat pomocí metody. Následující příklad načte všechny Blogy, jejich související příspěvky a autora každého příspěvku.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

Můžete zřetězit více volání, aby `ThenInclude` bylo možné pokračovat včetně dalších úrovní souvisejících dat.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

To všechno můžete zkombinovat, chcete-li zahrnout související data z více úrovní a více kořenů ve stejném dotazu.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

Možná budete chtít zahrnout několik souvisejících entit pro jednu z zahrnutých entit. Například při `Blogs`dotazování zahrnete `Author` `Posts` a potom chcete zahrnout `Tags` i z `Posts`. Chcete-li to provést, je třeba zadat každou cestu zahrnutí počínaje kořenovým adresářem. Například `Blog -> Posts -> Author` a `Blog -> Posts -> Tags`. To neznamená, že budete mít redundantní spojení. ve většině případů bude EF konsolidovat spojení při generování SQL.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

> [!CAUTION]
> Vzhledem k tomu, že verze 3.0.0, každý `Include` způsobí přidání dalších připojení k dotazům SQL vytvořeným relačními zprostředkovateli, zatímco předchozí verze vygenerovaly další dotazy SQL. To může významně změnit výkon dotazů pro lepší nebo horší. Konkrétně dotazy LINQ s větším vysokým počtem `Include` může být nutné rozdělit do několika samostatných dotazů LINQ, aby se předešlo problému s rozpadem kartézském.

### <a name="include-on-derived-types"></a>Zahrnout do odvozených typů

Můžete zahrnout související data z navigace definovaná pouze pro odvozený typ pomocí `Include` a. `ThenInclude` 

S ohledem na následující model:

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

`School` Obsah navigace všech uživatelů, kteří jsou studenti, se dá eagerly načíst pomocí několika vzorů:

- použití přetypování
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- operátor `as` using
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- použití přetížení `Include` , které přijímá parametr typu`string`
  ```csharp
  context.People.Include("School").ToList()
  ```

## <a name="explicit-loading"></a>explicitní načítání

Můžete explicitně načíst navigační vlastnost prostřednictvím `DbContext.Entry(...)` rozhraní API.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

Můžete také explicitně načíst vlastnost navigace spuštěním samostatného dotazu, který vrací související entity. Pokud je povoleno sledování změn, pak při načítání entity EF Core automaticky nastaví navigační vlastnosti nově načtených entity, aby odkazovaly na jakékoli již načtené entity, a nastavili navigační vlastnosti již načtených entit tak, aby odkazovaly na nově načtená entita

### <a name="querying-related-entities"></a>Dotazování souvisejících entit

Můžete také získat dotaz LINQ, který představuje obsah vlastnosti navigace.

To umožňuje provádět akce, jako je například spuštění agregačního operátoru nad souvisejícími entitami bez jejich načtení do paměti.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Můžete také filtrovat, které související entity jsou načteny do paměti.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>opožděné načítání

Nejjednodušším způsobem, jak použít opožděné načítání, je nainstalovat balíček [Microsoft. EntityFrameworkCore. proxy](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) a povolit mu volání `UseLazyLoadingProxies`. Příklad:

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
Nebo při použití AddDbContext:

```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```

EF Core pak povolí opožděné načítání pro jakoukoliv navigační vlastnost, která může být přepsána – to znamená, že `virtual` musí být a na třídě, která může být děděna z. Například v následujících entitách `Post.Blog` budou vlastnosti navigace a `Blog.Posts` aplikace opožděně načteny.

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

### <a name="lazy-loading-without-proxies"></a>Opožděné načítání bez proxy serverů

Opožděné načítání proxy serverů funguje tak, že `ILazyLoader` službu vloží do entity, jak je popsáno v tématu [konstruktory typu entity](../modeling/constructors.md). Příklad:

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

To nevyžaduje dědění typů entit nebo vlastností navigace jako virtuálních a umožňuje, aby se instance entit vytvořily s `new` opožděným načtením, a to po připojení k kontextu. Nicméně vyžaduje odkaz na `ILazyLoader` službu, která je definována v balíčku [Microsoft. EntityFrameworkCore. Abstracts](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) . Tento balíček obsahuje minimální sadu typů, aby v závislosti na tom byl velmi malý vliv. Pokud se však chcete zcela vyhnout v závislosti na EF Core balíčky v typech entit, je možné `ILazyLoader.Load` metodu vložit jako delegáta. Příklad:

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

Výše uvedený kód používá `Load` metodu rozšíření k použití delegáta bitového čističe:

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
> Parametr konstruktoru pro delegáta opožděného načítání musí být nazvaný "lazyLoader". Konfigurace, která má použít jiný název než toto je plánována v budoucí verzi.

## <a name="related-data-and-serialization"></a>Související data a serializace

Vzhledem k tomu, že EF Core bude automaticky opravovat navigační vlastnosti, můžete v grafu objektů končit cykly. Například načtení blogu a jeho souvisejících příspěvků bude mít za následek blogový objekt, který odkazuje na kolekci příspěvků. Každá z těchto příspěvků bude mít odkaz zpátky na blog.

Některé serializace rozhraní tyto cykly nepovolují. Json.NET například vyvolá následující výjimku, pokud dojde k cyklu.

> Newtonsoft. JSON. JsonSerializationException: Zjistila se smyčka s odkazem na vlastnost ' blog ' typu ' MyApplication. Models. blog '.

Pokud používáte ASP.NET Core, můžete Json.NET nakonfigurovat tak, aby ignorovala cykly, které najde v grafu objektů. To se provádí v `ConfigureServices(...)` metodě v. `Startup.cs`

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

Další možností je vyjednat jednu z navigačních vlastností s `[JsonIgnore]` atributem, který instruuje JSON.NET, že při serializaci nebude procházet tuto vlastnost navigace.
