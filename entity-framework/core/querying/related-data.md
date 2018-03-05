---
title: "Načítají se související Data – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: 0d7705e0e5368435536e98d319c853ea8c732643
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/05/2018
---
# <a name="loading-related-data"></a>Načítání související Data

Entity Framework Core umožňuje používat navigační vlastnosti v modelu se načíst související entity. Existují tři obecné vzory O/RM používají k zatížení související data.
* **Přes načítání** znamená, že související data načtená z databáze jako součást počáteční dotazu.
* **Explicitní načítání** znamená, že související data se explicitně načíst z databáze později.
* **Opožděného načítání** znamená, že související transparentně načtení dat z databáze při přístupu k navigační vlastnost.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.

## <a name="eager-loading"></a>přes načítání

Můžete použít `Include` metoda k určení související data mají být zahrnuty do výsledků dotazu. V následujícím příkladu, bude mít blogy, které jsou vráceny ve výsledcích jejich `Posts` vlastnost naplněný související příspěvky.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core bude automaticky opravu up navigačních vlastností pro ostatní entity, které byly dříve načteny do instance kontextu. Takže i v případě, že není výslovně zahrnout data pro navigační vlastnost, vlastnost pořád naplněný, pokud některé nebo všechny související entity byly dříve načteny.


Související data z více vztahů můžete zahrnout jeden dotaz.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Více úrovní

Podrobnostem přispívají vztahů s zahrnují více úrovní související data pomocí `ThenInclude` metoda. Následující příklad načte všechny blogy, jejich související příspěvky a Autor každé post.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Aktuální verze sady Visual Studio nabízí možnosti dokončení nesprávný kód a mohou způsobit správné výrazy označen příznakem chyby syntaxe při použití `ThenInclude` metoda po navigační vlastnost kolekce. Toto je příznakem IntelliSense chyb sledovány v https://github.com/dotnet/roslyn/issues/8237. Je bezpečné tyto chyby syntaxe nesprávné ignorovat, dokud je správný kód a mohou být zkompilovány úspěšně. 

Můžete zřetězit více volání `ThenInclude` Chcete-li pokračovat, včetně další úrovně související data.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Můžete sloučit všechny tohoto zahrnout související data z více úrovní a více kořenových certifikačních autorit ve stejném dotazu.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Můžete zahrnout více entit v relaci pro jednu z entity, které bude zahrnut. Například při dotazování `Blog`s, zahrnete `Posts` a chcete současně obsahovat `Author` a `Tags` z `Posts`. K tomuto účelu, budete muset zadat jednotlivé obsahovat cestu spouštění v kořenovém adresáři. Například `Blog -> Posts -> Author` a `Blog -> Posts -> Tags`. To neznamená, že budete mít redundantní spojení, ve většině případů, které budou konsolidovat EF spojení při generování SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>Zahrnout na odvozené typy

Můžete zahrnout související data z navigací definovaný jenom pro odvozený typ pomocí `Include` a `ThenInclude`. 

Zadaný model následující:

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

Obsah `School` navigační všichni uživatelé, kteří jsou studenti, kteří mohou být načteny například, s počtem vzorků:

- pomocí přetypování
```Csharp
context.People.Include(person => ((Student)person).School).ToList()
```

- pomocí `as` – operátor
```Csharp
context.People.Include(person => (person as Student).School).ToList()
```

- pomocí přetížení `Include` , která má parametr typu `string`
```Csharp
context.People.Include("Student").ToList()
```

### <a name="ignored-includes"></a>Ignorovat zahrnuje

Pokud změníte dotaz tak, že už vrátí instance typu entity, které začne dotaz s, se ignorují operátory zahrnout.

V následujícím příkladu jsou na základě operátory zahrnout `Blog`, ale pak `Select` operátor se používá ke změně dotaz vrátit instanci anonymního typu. V takovém případě operátory zahrnout nemají žádný vliv.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

Ve výchozím nastavení, bude EF základní zaprotokolovat upozornění při zahrnují operátory jsou ignorovány. V tématu [protokolování](../miscellaneous/logging.md) Další informace o zobrazení výstupu protokolování. Chování lze změnit, pokud zahrnout operátor je ignorován throw nebo nic nestane. To se provádí při nastavování možnosti pro váš kontext – obvykle ve `DbContext.OnConfiguring`, nebo v `Startup.cs` Pokud používáte ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>explicitní načítání

> [!NOTE]  
> Tato funkce byla zavedená v EF základní 1.1.

Můžete explicitně načíst navigační vlastnost prostřednictvím `DbContext.Entry(...)` rozhraní API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Můžete také explicitně načíst navigační vlastnost spuštěním další dotaz, který vrací související entity. Pokud je povoleno sledování změn, pak při načítání entitu, EF základní bude automaticky nastavit navigační vlastnosti nově načíst entitiy odkazovat na všechny entity již načten a vlastnosti navigace entit již načten k odkazování na nově načíst entity.

### <a name="querying-related-entities"></a>Dotazování entit v relaci

Můžete také získat LINQ dotazu, který reprezentuje obsah navigační vlastnosti.

To umožňuje provádět akce, jako je například spuštění agregační operátor přes související entity bez jejich načtení do paměti.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Můžete také filtrovat, které entit v relaci jsou načtena do paměti.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>opožděného načítání

> [!NOTE]  
> Tato funkce byla zavedená v EF základní 2.1.

Nejjednodušší způsob, jak používat opožděného načítání, je instalace [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) balíček a jeho povolení pomocí volání `UseLazyLoadingProxies`. Příklad:
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
Nebo pokud používáte AddDbContext:
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
Základní EF potom umožní opožděného načítání pro vlastnost navigace, která je možné přepsat – a který je, musí být `virtual` a na třídu, která je možné zdědit z. Například v následujících entit `Post.Blog` a `Blog.Posts` navigační vlastnosti bude opožděné načíst.
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
### <a name="lazy-loading-without-proxies"></a>Lazy načítání bez proxy

Lazy načítání proxy fungovat vložením `ILazyLoader` služby do entity, jak je popsáno v [konstruktory typu Entity](../modeling/constructors.md). Příklad:
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
        get => LazyLoader?.Load(this, ref _posts);
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
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
To nevyžaduje typy entit byla zděděna z a navigačních vlastností pro virtuální a umožňuje instancí entit, které jsou vytvořené pomocí `new` opožděné načtení jednou připojené k kontextu. To ale vyžaduje odkaz na `ILazyLoader` službu, která spáruje sestavení EF základní typy entit. Aby se zabránilo tento základní EF umožňuje `ILazyLoader.Load` metoda vložit jako delegáta. Příklad:
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
        get => LazyLoader?.Load(this, ref _posts);
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
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
Kód výše používá `Load` metoda rozšíření, aby pomocí delegát trochu čisticí:
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
> Parametr konstruktoru pro delegáta opožděného načítání musí být voláno "lazyLoader". Konfigurace použijte jiný název, který to je plánovaná pro budoucí použití.

## <a name="related-data-and-serialization"></a>Související data a serializace

Protože základní EF bude automaticky opravu up navigační vlastnosti, můžete skončili s cykly v grafu objektu. Například načítání blog a nesouvisí způsobí příspěvcích na blogu objekt, který odkazuje na kolekci zpráv. Každý z těchto příspěvcích bude mít odkaz na blogu.

Některé architektury serializace neumožňují takové cykly. Například Json.NET vyvolá následující výjimka, pokud se zjistil cyklický odkaz.

> Newtonsoft.Json.JsonSerializationException: Vlastní odkazující na smyčky zjištěna vlastnost 'Blog' typu 'MyApplication.Models.Blog'.

Pokud používáte ASP.NET Core, můžete nakonfigurovat Json.NET ignorovat cykly, které najde v grafu objektů. To se provádí v `ConfigureServices(...)` metoda v `Startup.cs`.

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
