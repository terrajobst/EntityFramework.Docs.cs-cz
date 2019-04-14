---
title: Načítají se související Data – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: e98e2e601203db7ea3d3344ddc7b7e0aff7f2143
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562517"
---
# <a name="loading-related-data"></a>Načítání souvisejících dat

Entity Framework Core umožňuje načtení souvisejících entit pomocí vlastnosti navigace v modelu. Existují tři běžné modely O/RM umožňují načíst související data.
* **Předběžné načítání** znamená, že je související data načtená z databáze jako součást počátečního dotazu.
* **Explicitní načtení** znamená, že související explicitní načtení dat z databáze později.
* **Opožděné načtení** znamená, že související transparentně načtení dat z databáze při přístupu k navigační vlastnost.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.

## <a name="eager-loading"></a>Předběžné načítání

Můžete použít `Include` metodu pro určení související data mají být zahrnuty ve výsledcích dotazu. V následujícím příkladu, blogy, které jsou vráceny ve výsledcích mají jejich `Posts` vlastnost naplní souvisejících příspěvků.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core se automaticky opravit navigační vlastnosti s jinými entitami, které byly dříve načtena do instance kontextu. Takže i v případě, že není explicitně zahrnout data pro navigační vlastnost, vlastnost pořád naplněný, pokud některé nebo všechny související entity byly dříve načteny.


Můžou obsahovat související data z více vztahů v jediném dotazu.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Více úrovní

Můžete procházet hierarchii prostřednictvím relace zahrnout související data s využitím více úrovní `ThenInclude` metody. Následující příklad načte všechny blogy, jejich souvisejících příspěvků a Autor každý příspěvek.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Aktuální verze sady Visual Studio nabízí možnosti dokončení nesprávný kód a může způsobit správné výrazy označen příznakem chyby syntaxe při použití `ThenInclude` za navigační vlastnost kolekce. Toto je příznakem chyby IntelliSense sledovány v https://github.com/dotnet/roslyn/issues/8237. Je bezpečné ignorovat tyto chyby syntaxe detekováno falešné jako kód je správný a může být zkompilován úspěšně. 

Můžete zřetězit více volání `ThenInclude` pokračovat, včetně dalších úrovní související data.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Můžete kombinovat, abyste mohli zahrnout související data z více úrovní a více kořenových adresářů ve stejném dotazu.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Můžete chtít zahrnout více souvisejících entit pro jeden z entity, které je zahrnuto. Třeba při dotazování `Blogs`, zahrnete `Posts` a poté chcete provést oba `Author` a `Tags` z `Posts`. Chcete-li to provést, musíte zadat jednotlivé obsahovat počínaje kořenovou cestu. Například `Blog -> Posts -> Author` a `Blog -> Posts -> Tags`. To neznamená, že se zobrazí redundantní spojení; ve většině případů bude EF konsolidovat spojení při generování SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>Umístit na odvozené typy

Můžete zahrnout související data z navigaci definovat jen pro odvozený typ pomocí `Include` a `ThenInclude`. 

Daný následující model:

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

Obsah `School` navigaci ve všech uživatelů, kteří studenty lze například načíst pomocí několika způsoby:

- pomocí přetypování
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- pomocí `as` – operátor
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- pomocí přetížení `Include` , která přebírá parametr typu `string`
  ```csharp
  context.People.Include("School").ToList()
  ```

### <a name="ignored-includes"></a>Ignorovat zahrnuje

Pokud změníte dotaz tak, aby by již nevracelo instancí typu entity, které se začnou dotazu, operátory zahrnout ignorovány.

V následujícím příkladu je na základě operátory zahrnout `Blog`, ale pak `Select` operátor se používá ke změně dotaz, který vrací anonymního typu. V takovém případě operátory zahrnout nemají žádný vliv.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

Ve výchozím nastavení, se budou protokolovat EF Core upozornění při operátory jsou ignorovány. Zobrazit [protokolování](../miscellaneous/logging.md) pro další informace o prohlížení uložit výstup protokolování. Můžete změnit chování při zahrnutí operátor ignorován provést operaci throw nebo Neprovádět žádnou akci. To se provádí při nastavování možnosti pro váš kontext – obvykle v `DbContext.OnConfiguring`, nebo v `Startup.cs` Pokud používáte ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>explicitní načtení

> [!NOTE]  
> Tato funkce byla zavedená v EF Core 1.1.

Můžete explicitně načíst navigační vlastnost via `DbContext.Entry(...)` rozhraní API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Navigační vlastnost můžete také explicitně načíst spuštěním samostatného dotaz, který vrátí související entity. Pokud je povoleno sledování změn, pak při načítání entity, EF Core automaticky nastavit vlastnosti navigace entitiy nově načíst k odkazování na všechny entity již načtena a nastavit vlastnosti navigace entit již načtena jako reference nově načtení entity.

### <a name="querying-related-entities"></a>Dotazování na související entity

Můžete také získat dotaz LINQ, který představuje obsah navigační vlastnost.

To umožňuje provádění akcí, například spuštění agregační operátor souvisejícími entitami bez jejich načtení do paměti.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Můžete také filtrovat, které související entity jsou načtena do paměti.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Opožděné načtení

> [!NOTE]  
> Tato funkce byla zavedená v EF Core 2.1.

Po instalaci je nejjednodušší způsob, jak pomocí opožděné načtení [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) balíček a jeho povolení voláním `UseLazyLoadingProxies`. Příklad:
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
EF Core se povolit opožděné načtení pro navigační vlastnost, která může být přepsána – to znamená, musí být `virtual` a na třídu, která je možné zdědit z. Například v následující entity `Post.Blog` a `Blog.Posts` navigační vlastnosti budou opožděné načtení.
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
### <a name="lazy-loading-without-proxies"></a>Opožděné načtení bez proxy servery

Opožděné načtení proxy servery fungují tak, že vkládá `ILazyLoader` služby do entity, jak je popsáno v [konstruktory typů entit](../modeling/constructors.md). Příklad:
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
To nevyžaduje, aby typy entit, aby se dědila z a navigačních vlastností pro virtuální a umožní instancí entit, které jsou vytvořené pomocí `new` opožděné načtení jednou připojené do kontextu. To ale vyžaduje odkaz na `ILazyLoader` služba, která je definována v [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) balíčku. Tento balíček obsahuje minimální sadu typů tak, aby se velmi malý vliv v ji. Pokud chcete úplně vyhnout v závislosti na všechny balíčky EF Core v typech entit, je však možné vložit `ILazyLoader.Load` metoda jako delegát. Příklad:
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
Kód výše používá `Load` metodu rozšíření k trochu čisticí využitím delegáta:
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
> Parametr konstruktoru pro opožděné načtení delegáta se musí volat "lazyLoader". Konfigurace, aby používala jiný název než to je plánovaná pro budoucí verzi.

## <a name="related-data-and-serialization"></a>Serializace a souvisejících dat

Protože EF Core se automaticky opravit navigačních vlastností můžete skončit s cykly v grafu objektu. Například načítání blogu a jeho souvisejících příspěvků výsledkem bude objekt blogu, který odkazuje na kolekci příspěvků. Každá z těchto míst bude mít odkaz na blogu.

Některé architektury serializace neumožňují takové cykly. Například Json.NET vyvolá následující výjimku, pokud dochází k zacyklení.

> Newtonsoft.Json.JsonSerializationException: Vlastní odkazující na zjištěna pro vlastnost "Blogu" typu "MyApplication.Models.Blog" smyčka.

Pokud používáte ASP.NET Core, můžete nakonfigurovat Json.NET ignorovat cykly, které najde v grafu objektů. To se provádí v `ConfigureServices(...)` metoda ve `Startup.cs`.

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
