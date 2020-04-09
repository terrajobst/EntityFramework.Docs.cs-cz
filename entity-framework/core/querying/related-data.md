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
# <a name="loading-related-data"></a>Načítání souvisejících dat

Entity Framework Core umožňuje použít navigační vlastnosti v modelu k načtení souvisejících entit. K načtení souvisejících dat se používají tři běžné vzory O/RM.

* **Dychtivé načítání** znamená, že související data se načtou z databáze jako součást počátečního dotazu.
* **Explicitní načítání** znamená, že související data jsou explicitně načtena z databáze později.
* **Opožděné načítání** znamená, že související data jsou transparentně načtena z databáze při přístupu k vlastnosti navigace.

> [!TIP]  
> Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) můžete zobrazit na GitHubu.

## <a name="eager-loading"></a>Dychtivé načítání

Tuto metodu `Include` můžete použít k určení souvisejících dat, která mají být zahrnuta do výsledků dotazu. V následujícím příkladu blogy, které jsou vráceny `Posts` ve výsledcích bude mít jejich vlastnost naplněna související příspěvky.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Jádro frameworku entit automaticky opraví navigační vlastnosti na všechny ostatní entity, které byly dříve načteny do instance kontextu. Takže i v případě, že explicitně nezahrnete data pro navigační vlastnost, vlastnost může být stále naplněna, pokud byly dříve načteny některé nebo všechny související entity.

Do jednoho dotazu můžete zahrnout související data z více relací.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Včetně více úrovní

Pomocí `ThenInclude` metody můžete procházet relacemi a zahrnout více úrovní souvisejících dat. Následující příklad načte všechny blogy, jejich související příspěvky a autora každého příspěvku.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

Můžete zřetězit `ThenInclude` více volání pokračovat, včetně další úrovně souvisejících dat.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

To vše můžete zkombinovat tak, aby zahrnovala související data z více úrovní a více kořenů ve stejném dotazu.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

Můžete chtít zahrnout více souvisejících entit pro jednu z entit, které jsou zahrnuty. Například `Blogs`při dotazování , `Posts` zahrnout a potom `Author` chcete `Tags` zahrnout `Posts`jak a a . Chcete-li to provést, je třeba zadat každou cestu zahrnutí začínající v kořenovém adresáři. Například `Blog -> Posts -> Author` a `Blog -> Posts -> Tags`. To neznamená, že dostanete nadbytečné spojení; ve většině případů EF bude konsolidovat spojení při generování SQL.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

> [!CAUTION]
> Od verze 3.0.0, každý `Include` způsobí další JOIN přidat do dotazů SQL vytvářených relační zprostředkovatelé, zatímco předchozí verze generované další dotazy SQL. To může výrazně změnit výkon dotazů, v dobrém i ve zlém. Zejména linq dotazy s mimořádně vysokým počtem `Include` operátorů může být nutné rozdělit do více samostatných linq dotazy, aby se zabránilo problém s kartézním rozpadem.

### <a name="include-on-derived-types"></a>Zahrnout na odvozené typy

Související data z navigace můžete zahrnout pouze na `Include` odvozeném typu pomocí a `ThenInclude`.

Vzhledem k následujícímu modelu:

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

Obsah `School` navigace všech lidí, kteří jsou studenty, lze dychtivě načíst pomocí několika vzorů:

* použití přetypážení

  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

* pomocí `as` operátoru

  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

* použití `Include` přetížení, které má parametr typu`string`

  ```csharp
  context.People.Include("School").ToList()
  ```

## <a name="explicit-loading"></a>Explicitní načítání

Můžete explicitně načíst navigační `DbContext.Entry(...)` vlastnost prostřednictvím rozhraní API.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

Můžete také explicitně načíst vlastnost navigace spuštěním samostatného dotazu, který vrací související entity. Pokud je povoleno sledování změn, pak při načítání entity EF Core automaticky nastaví navigační vlastnosti nově načtené entity tak, aby odkazovaly na všechny entity, které již byly načteny, a nastaví navigační vlastnosti již načtených entit tak, aby odkazovaly na nově načtenou entitu.

### <a name="querying-related-entities"></a>Dotazování souvisejících entit

Můžete také získat dotaz LINQ, který představuje obsah navigační vlastnosti.

To umožňuje například spuštění agregačního operátoru přes související entity bez jejich načtení do paměti.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Můžete také filtrovat, které související entity jsou načteny do paměti.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Opožděné načtení

Nejjednodušší způsob, jak používat opožděné načítání je instalací balíčku [Microsoft.EntityFrameworkCore.Proxy](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) a `UseLazyLoadingProxies`povolení s voláním . Příklad:

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

EF Core pak povolí opožděné načítání pro všechny navigační vlastnosti, `virtual` které mohou být přepsány -- to znamená, že musí být a na třídu, která může být zděděna z. Například v následujících `Post.Blog` entitách `Blog.Posts` a navigační vlastnosti budou opožděně načteny.

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

Opožděné načítání proxy servery práce `ILazyLoader` vstřikováním služby do entity, jak je popsáno v [konstruktory typu entity](../modeling/constructors.md). Příklad:

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

To nevyžaduje, aby typy entit byly zděděny nebo navigační vlastnosti `new` byly virtuální, a umožňuje instance entit vytvořené s opožděným načtením, jakmile jsou připojeny k kontextu. Vyžaduje však odkaz na `ILazyLoader` službu, která je definována v balíčku [Microsoft.EntityFrameworkCore.Abstractions.](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) Tento balíček obsahuje minimální sadu typů tak, aby v závislosti na něm byl velmi malý dopad. Chcete-li se však zcela vyhnout v závislosti na všech balíčcích EF Core v typech entit, je možné vložit metodu `ILazyLoader.Load` jako delegát. Příklad:

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

Výše uvedený kód `Load` používá metodu rozšíření, aby se pomocí delegáta trochu čistší:

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
> Parametr konstruktoru pro delegáta s opožděným načítáním se musí nazývat "lazyLoader". Konfigurace pro použití jiného názvu, než je plánováno pro budoucí verzi.

## <a name="related-data-and-serialization"></a>Související data a serializace

Vzhledem k tomu, že EF Core automaticky opraví navigační vlastnosti, můžete skončit s cykly v grafu objektu. Například načtení blogu a jeho souvisejících příspěvků bude mít za následek objekt blogu, který odkazuje na kolekci příspěvků. Každý z těchto příspěvků bude mít odkaz zpět na blog.

Některé architektury serializace neumožňují takové cykly. Například Json.NET vyvolá následující výjimku, pokud dojde k cyklu.

> Newtonsoft.Json.JsonSerializationException: Self referencing smyčky zjištěna pro vlastnost 'Blog' s typem 'MyApplication.Models.Blog'.

Pokud používáte ASP.NET Core, můžete nakonfigurovat Json.NET ignorovat cykly, které najde v grafu objektu. To se provádí `ConfigureServices(...)` v `Startup.cs`metodě v .

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

Další alternativou je ozdobit jednu `[JsonIgnore]` z navigačních vlastností atributem, který Json.NET neprocházet tuto vlastnost navigace při serializaci.
