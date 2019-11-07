---
title: Načítají se související data – EF Core.
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: bfabe8fd5b0a64edd5d97baff3beab9d712f1c20
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654636"
---
# <a name="loading-related-data"></a>Načítání souvisejících dat

Entity Framework Core umožňuje v modelu použít navigační vlastnosti pro načtení souvisejících entit. Existují tři běžné vzory O/RM, které slouží k načtení souvisejících dat.

* **Eager načítání** znamená, že související data jsou načtena z databáze jako součást počátečního dotazu.
* **Explicitní načítání** znamená, že související data jsou explicitně načtena z databáze později.
* **Opožděné načítání** znamená, že související data jsou transparentně načítána z databáze, pokud je k ní přistupovaná vlastnost navigace.

> [!TIP]  
> [Ukázku](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) tohoto článku můžete zobrazit na GitHubu.

## <a name="eager-loading"></a>Eager načítání

Pomocí metody `Include` můžete určit související data, která se mají zahrnout do výsledků dotazu. V následujícím příkladu budou Blogy, které jsou vráceny ve výsledcích, obsahovat jejich `Posts` vlastnost naplněná souvisejícími příspěvky.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core bude automaticky opravovat navigační vlastnosti pro všechny další entity, které byly dříve načteny do instance kontextu. Takže i když nebudete explicitně vkládat data pro navigační vlastnost, může být tato vlastnost i nadále naplněna, pokud byly některé nebo všechny související entity dříve načteny.

V jednom dotazu můžete zahrnout související data z několika vztahů.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Zahrnutí více úrovní

Pomocí metody `ThenInclude` můžete procházet hierarchii a zahrnout do nich více úrovní souvisejících dat. Následující příklad načte všechny Blogy, jejich související příspěvky a autora každého příspěvku.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

Můžete zřetězit více volání `ThenInclude`, abyste mohli pokračovat, včetně dalších úrovní souvisejících dat.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

To všechno můžete zkombinovat, chcete-li zahrnout související data z více úrovní a více kořenů ve stejném dotazu.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

Možná budete chtít zahrnout několik souvisejících entit pro jednu z zahrnutých entit. Při dotazování `Blogs`například zahrnete `Posts` a pak chcete zahrnout `Author` i `Tags` `Posts`. Chcete-li to provést, je třeba zadat každou cestu zahrnutí počínaje kořenovým adresářem. Například `Blog -> Posts -> Author` a `Blog -> Posts -> Tags`. To neznamená, že budete mít redundantní spojení. ve většině případů bude EF konsolidovat spojení při generování SQL.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

> [!CAUTION]
> Vzhledem k tomu, že verze 3.0.0, každá `Include` způsobí přidání dalších připojení k dotazům SQL vytvořeným relačními zprostředkovateli, zatímco předchozí verze vygenerovaly další dotazy SQL. To může významně změnit výkon dotazů pro lepší nebo horší. Konkrétně dotazy LINQ s větším počtem `Include`ch operátorů může být nutné rozdělit do několika samostatných dotazů LINQ, aby se zabránilo problému s vyvýbuchou kartézském.

### <a name="include-on-derived-types"></a>Zahrnout do odvozených typů

Můžete zahrnout související data z navigace definovaná pouze pro odvozený typ pomocí `Include` a `ThenInclude`.

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

Obsah `School` navigace všech lidí, kteří Studenté můžou načítat eagerly pomocí několika vzorů:

* použití přetypování

  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

* použití operátoru `as`

  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

* použití přetížení `Include`, které přebírá parametr typu `string`

  ```csharp
  context.People.Include("School").ToList()
  ```

## <a name="explicit-loading"></a>Explicitní načítání

Můžete explicitně načíst navigační vlastnost prostřednictvím rozhraní `DbContext.Entry(...)` API.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

Můžete také explicitně načíst vlastnost navigace spuštěním samostatného dotazu, který vrací související entity. Pokud je povoleno sledování změn, pak při načítání entity EF Core automaticky nastaví navigační vlastnosti nově načtených entity, aby odkazovaly na jakékoli již načtené entity, a nastavili navigační vlastnosti již načtených entit tak, aby odkazovaly na nově načtená entita

### <a name="querying-related-entities"></a>Dotazování souvisejících entit

Můžete také získat dotaz LINQ, který představuje obsah vlastnosti navigace.

To umožňuje provádět akce, jako je například spuštění agregačního operátoru nad souvisejícími entitami bez jejich načtení do paměti.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Můžete také filtrovat, které související entity jsou načteny do paměti.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Opožděné načítání

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

EF Core pak povolí opožděné načítání pro jakoukoliv navigační vlastnost, která může být přepsána – to znamená, že musí být `virtual` a na třídě, která může být děděna z. Například v následujících entitách budou vlastnosti `Post.Blog` a `Blog.Posts` navigace opožděně načteny.

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

Opožděné načítání proxy serverů funguje tak, že se služba `ILazyLoader` vloží do entity, jak je popsáno v tématu [konstruktory typu entity](../modeling/constructors.md). Příklad:

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

Tato možnost nepožaduje dědění typů entit nebo vlastností navigace jako virtuálních a umožňuje, aby se instance entit vytvořily pomocí `new` opožděně načteny po připojení k kontextu. Nicméně vyžaduje odkaz na službu `ILazyLoader`, která je definována v balíčku [Microsoft. EntityFrameworkCore. Abstracts](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) . Tento balíček obsahuje minimální sadu typů, aby v závislosti na tom byl velmi malý vliv. Pokud se však chcete zcela vyhnout v závislosti na jakémkoli EF Core balíčky v typech entit, je možné metodu `ILazyLoader.Load` vložit jako delegáta. Příklad:

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

Výše uvedený kód používá metodu rozšíření `Load` k použití delegáta bitového čističe:

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

> Newtonsoft. JSON. JsonSerializationException: byla zjištěna smyčka s odkazem na vlastnost ' blog ' s typem ' MyApplication. Models. blog '.

Pokud používáte ASP.NET Core, můžete Json.NET nakonfigurovat tak, aby ignorovala cykly, které najde v grafu objektů. To se provádí v metodě `ConfigureServices(...)` v `Startup.cs`.

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

Další možností je vyjednat jednu z navigačních vlastností pomocí atributu `[JsonIgnore]`, který dává pokyn, aby při serializaci Json.NET, že se tato vlastnost navigace neprojde.
