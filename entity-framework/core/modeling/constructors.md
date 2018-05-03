---
title: Typy entit pomocí konstruktorů - EF jádra
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 8cea624c295f99ef54cb8b4758642eade03c235e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="bbc20-102">Typy entit pomocí konstruktorů</span><span class="sxs-lookup"><span data-stu-id="bbc20-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="bbc20-103">Tato funkce je nového v EF základní 2.1.</span><span class="sxs-lookup"><span data-stu-id="bbc20-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="bbc20-104">Od verze 2.1 základní EF, je nyní možné určit konstruktor s parametry a základní EF volání tento konstruktor při vytváření instance entity.</span><span class="sxs-lookup"><span data-stu-id="bbc20-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="bbc20-105">Konstruktor parametry mohou být vázány na namapované vlastnosti, nebo na různé typy služeb pro usnadnění jako chování opožděného načítání.</span><span class="sxs-lookup"><span data-stu-id="bbc20-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="bbc20-106">Od verze 2.1 základní EF všechny konstruktor vazba je pomocí konvence.</span><span class="sxs-lookup"><span data-stu-id="bbc20-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="bbc20-107">Konfigurace konkrétní konstruktory používat je plánovaná pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="bbc20-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="bbc20-108">Vytvoření vazby na namapované vlastnosti</span><span class="sxs-lookup"><span data-stu-id="bbc20-108">Binding to mapped properties</span></span>

<span data-ttu-id="bbc20-109">Vezměte v úvahu typické modelu příspěvku na blogu:</span><span class="sxs-lookup"><span data-stu-id="bbc20-109">Consider a typical Blog/Post model:</span></span>

```Csharp
public class Blog
{
    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="bbc20-110">Když EF základní vytváří instance tyto typy, jako výsledků dotazu, bude nejprve volat výchozí konstruktor bez parametrů a pak každou vlastnost nastavena na hodnotu z databáze.</span><span class="sxs-lookup"><span data-stu-id="bbc20-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="bbc20-111">Ale pokud EF základní vyhledá parametrizované konstruktor s názvy parametrů a typy, které se shodují mapovat vlastnosti a potom ho bude místo toho volat parametrizované konstruktor s hodnotami pro tyto vlastnosti a nebude explicitně nastaven každou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bbc20-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="bbc20-112">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bbc20-112">For example:</span></span>

```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
<span data-ttu-id="bbc20-113">Některé věci Všimněte si:</span><span class="sxs-lookup"><span data-stu-id="bbc20-113">Some things to note:</span></span>
* <span data-ttu-id="bbc20-114">Ne všechny vlastnosti musí být parametry konstruktor.</span><span class="sxs-lookup"><span data-stu-id="bbc20-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="bbc20-115">Například vlastnost Post.Content není nastaven libovolný parametr konstruktoru, takže EF základní ho nastavit po volání metody konstruktoru běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="bbc20-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="bbc20-116">Typy parametrů a názvy shodovat typy vlastností a názvy, s tím rozdílem, že vlastnosti může být použita Pascal parametry jsou ve formátu camelCase.</span><span class="sxs-lookup"><span data-stu-id="bbc20-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="bbc20-117">Základní EF nelze nastavit vlastnosti navigace (například blogu nebo příspěvky výše) pomocí konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="bbc20-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="bbc20-118">Konstruktor může být veřejné, privátní, nebo jiné usnadnění.</span><span class="sxs-lookup"><span data-stu-id="bbc20-118">The constructor can be public, private, or have any other accessibility.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="bbc20-119">Vlastnosti jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="bbc20-119">Read-only properties</span></span>

<span data-ttu-id="bbc20-120">Jakmile se nastaveny vlastnosti prostřednictvím konstruktoru ho mít smysl Chcete-li některá z nich jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="bbc20-120">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="bbc20-121">Základní EF to podporuje, ale některé kroky, chcete-li o:</span><span class="sxs-lookup"><span data-stu-id="bbc20-121">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="bbc20-122">Vlastnosti bez setter nejsou mapovat pomocí konvence.</span><span class="sxs-lookup"><span data-stu-id="bbc20-122">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="bbc20-123">(Díky tomu se obvykle mapy – vlastnosti, které nelze mapovat, jako je například počítané vlastnosti.)</span><span class="sxs-lookup"><span data-stu-id="bbc20-123">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="bbc20-124">Použití automaticky generované hodnoty klíče vyžaduje klíčové vlastnosti, která je pro čtení a zápis, protože hodnota klíče je třeba nastavit klíče generátorem při vkládání nové entity.</span><span class="sxs-lookup"><span data-stu-id="bbc20-124">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="bbc20-125">Snadný způsob, jak se vyhnout tyto akce se má používat privátní setter.</span><span class="sxs-lookup"><span data-stu-id="bbc20-125">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="bbc20-126">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bbc20-126">For example:</span></span>
```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; private set; }

    public string Name { get; private set; }
    public string Author { get; private set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; private set; }

    public string Title { get; private set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; private set; }

    public Blog Blog { get; set; }
}
```
<span data-ttu-id="bbc20-127">Základní EF uvidí vlastnost s privátní setter jako pro čtení a zápis, což znamená, že jsou jako předtím namapované všechny vlastnosti a klíč můžete stále být generovaný úložištěm.</span><span class="sxs-lookup"><span data-stu-id="bbc20-127">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="bbc20-128">Alternativu k použití privátního setter je nastavení vlastnosti skutečně jen pro čtení a přidání více explicitní mapování v OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="bbc20-128">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="bbc20-129">Některé vlastnosti, můžete úplně odstraněny a nahradí pouze pole.</span><span class="sxs-lookup"><span data-stu-id="bbc20-129">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="bbc20-130">Představte si třeba tyto typy entit:</span><span class="sxs-lookup"><span data-stu-id="bbc20-130">For example, consider these entity types:</span></span>

```Csharp
public class Blog
{
    private int _id;

    public Blog(string name, string author)
    {
        Name = name;
        Author = author;
    }

    public string Name { get; }
    public string Author { get; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    private int _id;

    public Post(string title, DateTime postedOn)
    {
        Title = title;
        PostedOn = postedOn;
    }

    public string Title { get; }
    public string Content { get; set; }
    public DateTime PostedOn { get; }

    public Blog Blog { get; set; }
}
```
<span data-ttu-id="bbc20-131">A tato konfigurace v OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="bbc20-131">And this configuration in OnModelCreating:</span></span>
```Csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Author);
            b.Property(e => e.Name);
        });

    modelBuilder.Entity<Post>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Title);
            b.Property(e => e.PostedOn);
        });
}
```
<span data-ttu-id="bbc20-132">Co je potřeba Všimněte si:</span><span class="sxs-lookup"><span data-stu-id="bbc20-132">Things to note:</span></span>
* <span data-ttu-id="bbc20-133">Pole je nyní klíč "vlastnost".</span><span class="sxs-lookup"><span data-stu-id="bbc20-133">The key "property" is now a field.</span></span> <span data-ttu-id="bbc20-134">Není `readonly` pole tak, aby bylo možné použít klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="bbc20-134">It is not a `readonly` field so that store-generated keys can be used.</span></span> 
* <span data-ttu-id="bbc20-135">Ostatní vlastnosti jsou jen pro čtení vlastnosti nastavit pouze v konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="bbc20-135">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="bbc20-136">Pokud hodnotu primárního klíče je vždy jen nastaven EF ani načíst z databáze, je potřeba zahrnout do konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="bbc20-136">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="bbc20-137">Tím zůstane klíč "vlastnost" jako jednoduchý pole a jasně ukazuje, že ho by neměla být explicitně nastaveno při vytváření nové blogy nebo příspěvky.</span><span class="sxs-lookup"><span data-stu-id="bbc20-137">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="bbc20-138">Tento kód bude mít za následek kompilátoru upozornění '169' indikující, že pole se nikdy nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="bbc20-138">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="bbc20-139">To můžete ignorovat, protože ve skutečnosti EF základní používá pole extralinguistic způsobem.</span><span class="sxs-lookup"><span data-stu-id="bbc20-139">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="bbc20-140">Vložení služby</span><span class="sxs-lookup"><span data-stu-id="bbc20-140">Injecting services</span></span>

<span data-ttu-id="bbc20-141">Základní EF můžete také vložit "služby" do konstruktoru typu entity.</span><span class="sxs-lookup"><span data-stu-id="bbc20-141">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="bbc20-142">Například následující může vložit:</span><span class="sxs-lookup"><span data-stu-id="bbc20-142">For example, the following can be injected:</span></span>
* <span data-ttu-id="bbc20-143">`DbContext` -aktuální instance kontextu, které mohou být zadány jako vaše odvozený typ DbContext</span><span class="sxs-lookup"><span data-stu-id="bbc20-143">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="bbc20-144">`ILazyLoader` -Služba opožděného načítání--najdete v článku [opožděného načítání dokumentaci](../querying/related-data.md) další podrobnosti</span><span class="sxs-lookup"><span data-stu-id="bbc20-144">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="bbc20-145">`Action<object, string>` -opožděného načítání delegáta – najdete v článku [opožděného načítání dokumentaci](../querying/related-data.md) další podrobnosti</span><span class="sxs-lookup"><span data-stu-id="bbc20-145">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="bbc20-146">`IEntityType` -metadata EF základní přidružené tomuto typu entity</span><span class="sxs-lookup"><span data-stu-id="bbc20-146">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="bbc20-147">Od verze 2.1 základní EF může vložit pouze služby, známého EF jádra.</span><span class="sxs-lookup"><span data-stu-id="bbc20-147">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="bbc20-148">Podpora pro vložení aplikační služby se považuje za pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="bbc20-148">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="bbc20-149">Například vloženého DbContext slouží pro selektivní přístup k databázi chcete získat informace o entit v relaci bez načítání je všechny.</span><span class="sxs-lookup"><span data-stu-id="bbc20-149">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="bbc20-150">V následujícím příkladu se používá získat počet příspěvky v blogu bez načítání v příspěvcích:</span><span class="sxs-lookup"><span data-stu-id="bbc20-150">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

```Csharp
public class Blog
{
    public Blog()
    {
    }

    private Blog(BloggingContext context)
    {
        Context = context;
    }

    private BloggingContext Context { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; set; }

    public int PostsCount
        => Posts?.Count
           ?? Context?.Set<Post>().Count(p => Id == EF.Property<int?>(p, "BlogId"))
           ?? 0;
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
<span data-ttu-id="bbc20-151">Všimněte si o tom několik akcí:</span><span class="sxs-lookup"><span data-stu-id="bbc20-151">A few things to notice about this:</span></span>
* <span data-ttu-id="bbc20-152">Konstruktor je soukromé, protože pouze někdy je volána metodou EF jádra a je jiný veřejný konstruktor pro obecné použití.</span><span class="sxs-lookup"><span data-stu-id="bbc20-152">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="bbc20-153">Kód pomocí služby vloženého (tj. v kontextu) je Obranným u ní se `null` pro zpracování případů, kde není EF základní vytvořit instanci.</span><span class="sxs-lookup"><span data-stu-id="bbc20-153">The code using the injected service (i.e. the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="bbc20-154">Protože služba je uložené v vlastnost pro čtení a zápis se resetuje po připojení k nové instance kontextu entity.</span><span class="sxs-lookup"><span data-stu-id="bbc20-154">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="bbc20-155">Vložení DbContext takto často považuje za proti vzor vzhledem k tomu, že ho páry v odstupu vaší typy entit přímo na EF jádra.</span><span class="sxs-lookup"><span data-stu-id="bbc20-155">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="bbc20-156">Pečlivě zvažte všechny možnosti před použitím služby vkládání takto.</span><span class="sxs-lookup"><span data-stu-id="bbc20-156">Carefully consider all options before using service injection like this.</span></span>
