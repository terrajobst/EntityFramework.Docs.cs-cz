---
title: Typy entit s konstruktory – EF Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: 1b36197465fb9a6571a306d36eb1e9d885a5399e
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152462"
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="da5c1-102">Typy entit s konstruktory</span><span class="sxs-lookup"><span data-stu-id="da5c1-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="da5c1-103">Tato funkce je nového v EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="da5c1-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="da5c1-104">Od verze EF Core 2.1, je teď možné definovat konstruktor s parametry a EF Core volání tohoto konstruktoru při vytváření instance entity.</span><span class="sxs-lookup"><span data-stu-id="da5c1-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="da5c1-105">Parametry konstruktoru mohou být vázány na připojené vlastnosti nebo na různé typy služeb, aby se usnadnilo chování jako opožděné načtení.</span><span class="sxs-lookup"><span data-stu-id="da5c1-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="da5c1-106">EF Core 2.1 všechny vazby konstruktor je podle konvence.</span><span class="sxs-lookup"><span data-stu-id="da5c1-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="da5c1-107">Konfigurace specifické konstruktory používat pro budoucí verzi plánujeme přidat.</span><span class="sxs-lookup"><span data-stu-id="da5c1-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="da5c1-108">Vazba pro mapovanou vlastnosti</span><span class="sxs-lookup"><span data-stu-id="da5c1-108">Binding to mapped properties</span></span>

<span data-ttu-id="da5c1-109">Zvažte typické modelu/blogu:</span><span class="sxs-lookup"><span data-stu-id="da5c1-109">Consider a typical Blog/Post model:</span></span>

``` csharp
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

<span data-ttu-id="da5c1-110">Když EF Core vytváří instance typů, například pro výsledky dotazu, bude nejprve volat výchozí konstruktor a nastavte jednotlivé vlastnosti na hodnotu z databáze.</span><span class="sxs-lookup"><span data-stu-id="da5c1-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="da5c1-111">Ale pokud najde konstruktorem s EF Core mapovat názvy parametrů a typy, které odpovídají vlastnosti a pak bude místo toho volat Parametrizovaný konstruktor s hodnotami pro tyto vlastnosti a není explicitně nastavena každou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="da5c1-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="da5c1-112">Příklad:</span><span class="sxs-lookup"><span data-stu-id="da5c1-112">For example:</span></span>

``` csharp
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
<span data-ttu-id="da5c1-113">Poznamenat několik věcí:</span><span class="sxs-lookup"><span data-stu-id="da5c1-113">Some things to note:</span></span>
* <span data-ttu-id="da5c1-114">Ne všechny vlastnosti musí mít parametry konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="da5c1-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="da5c1-115">Například vlastnost Post.Content není nastavena podle jakékoli parametr konstruktoru, takže EF Core ho nastavte po volání konstruktoru běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="da5c1-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="da5c1-116">Typy parametrů a názvů musí odpovídat názvy a typy vlastností s tím rozdílem, že vlastnosti mohou být – jazyka Pascal – parametry jsou – ve formátu camelCase.</span><span class="sxs-lookup"><span data-stu-id="da5c1-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="da5c1-117">EF Core nelze nastavit navigační vlastnosti (například blogu nebo příspěvky výše) pomocí konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="da5c1-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="da5c1-118">Konstruktor může být veřejný, privátní, nebo máte další usnadnění přístupu.</span><span class="sxs-lookup"><span data-stu-id="da5c1-118">The constructor can be public, private, or have any other accessibility.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="da5c1-119">Vlastnosti jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="da5c1-119">Read-only properties</span></span>

<span data-ttu-id="da5c1-120">Jakmile se nastavují vlastnosti prostřednictvím konstruktoru může být vhodné provést některé z nich jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="da5c1-120">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="da5c1-121">EF Core podporuje, ale k vyhledání navýšení kapacity na několik věcí:</span><span class="sxs-lookup"><span data-stu-id="da5c1-121">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="da5c1-122">Vlastnosti bez setter nejsou mapovat pomocí konvence.</span><span class="sxs-lookup"><span data-stu-id="da5c1-122">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="da5c1-123">(To se obvykle mapy – vlastnosti, které by neměly být mapována, například vypočítané vlastnosti.)</span><span class="sxs-lookup"><span data-stu-id="da5c1-123">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="da5c1-124">Použití hodnoty automaticky generovaného klíče vyžaduje klíčová vlastnost, která je pro čtení i zápis, protože hodnota klíče, která se musí nastavit generátorem klíče při vložení nových entit.</span><span class="sxs-lookup"><span data-stu-id="da5c1-124">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="da5c1-125">Snadný způsob, jak se vyhnout tyto věci se má používat soukromé funkce setter.</span><span class="sxs-lookup"><span data-stu-id="da5c1-125">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="da5c1-126">Příklad:</span><span class="sxs-lookup"><span data-stu-id="da5c1-126">For example:</span></span>
``` csharp
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
<span data-ttu-id="da5c1-127">EF Core považuje čtení i zápis, což znamená, že všechny vlastnosti jsou mapovány stejně jako předtím a klíč se můžete stále možné generovaných úložištěm vlastnost s privátní metodu setter.</span><span class="sxs-lookup"><span data-stu-id="da5c1-127">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="da5c1-128">Alternativou k používání soukromý setter je nastavení vlastnosti skutečně jen pro čtení a přidat více explicitního mapování v OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="da5c1-128">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="da5c1-129">Některé vlastnosti, můžete k úplnému odebrání a nahradí pouze pole.</span><span class="sxs-lookup"><span data-stu-id="da5c1-129">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="da5c1-130">Představte si třeba tyto typy entit:</span><span class="sxs-lookup"><span data-stu-id="da5c1-130">For example, consider these entity types:</span></span>

``` csharp
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
<span data-ttu-id="da5c1-131">A tato konfigurace v OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="da5c1-131">And this configuration in OnModelCreating:</span></span>
``` csharp
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
<span data-ttu-id="da5c1-132">Co je potřeba mějte na paměti:</span><span class="sxs-lookup"><span data-stu-id="da5c1-132">Things to note:</span></span>
* <span data-ttu-id="da5c1-133">Pole je nyní klíč "vlastnosti".</span><span class="sxs-lookup"><span data-stu-id="da5c1-133">The key "property" is now a field.</span></span> <span data-ttu-id="da5c1-134">Nejedná se `readonly` pole tak, aby klíče generované úložištěm můžete použít.</span><span class="sxs-lookup"><span data-stu-id="da5c1-134">It is not a `readonly` field so that store-generated keys can be used.</span></span>
* <span data-ttu-id="da5c1-135">Další vlastnosti jsou jen pro čtení vlastnosti nastavené jenom v konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="da5c1-135">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="da5c1-136">Pokud hodnotu primárního klíče je vždy jen nastavit EF nebo čtení z databáze, je potřeba zahrnout do konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="da5c1-136">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="da5c1-137">To opustí klíč "vlastnosti" jako jednoduchý pole a je zřejmé, že ho by neměla být explicitně nastaveno při vytváření nových blogů a příspěvky.</span><span class="sxs-lookup"><span data-stu-id="da5c1-137">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="da5c1-138">Tento kód způsobí kompilátor varování "169" označující, že pole se nikdy nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="da5c1-138">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="da5c1-139">To můžete ignorovat, protože ve skutečnosti je EF Core pomocí pole extralinguistic způsobem.</span><span class="sxs-lookup"><span data-stu-id="da5c1-139">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="da5c1-140">Vkládání služeb</span><span class="sxs-lookup"><span data-stu-id="da5c1-140">Injecting services</span></span>

<span data-ttu-id="da5c1-141">EF Core můžete také vložit "služby" do konstruktoru typu entity.</span><span class="sxs-lookup"><span data-stu-id="da5c1-141">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="da5c1-142">Například následující může být vloženy:</span><span class="sxs-lookup"><span data-stu-id="da5c1-142">For example, the following can be injected:</span></span>
* <span data-ttu-id="da5c1-143">`DbContext` -aktuální instance kontextu, které mohou být zadány jako odvozený typ DbContext</span><span class="sxs-lookup"><span data-stu-id="da5c1-143">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="da5c1-144">`ILazyLoader` -opožděné načtení služby – najdete v článku [opožděné načtení dokumentaci](../querying/related-data.md) další podrobnosti</span><span class="sxs-lookup"><span data-stu-id="da5c1-144">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="da5c1-145">`Action<object, string>` -opožděné načtení delegáta--najdete v článku [opožděné načtení dokumentaci](../querying/related-data.md) další podrobnosti</span><span class="sxs-lookup"><span data-stu-id="da5c1-145">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="da5c1-146">`IEntityType` -EF Core metadata přidružená k tomuto typu entity</span><span class="sxs-lookup"><span data-stu-id="da5c1-146">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="da5c1-147">Od verze EF Core 2.1 lze vloženy pouze služeb, které EF Core.</span><span class="sxs-lookup"><span data-stu-id="da5c1-147">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="da5c1-148">Podpora pro vkládání aplikace služby se považují za pro budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="da5c1-148">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="da5c1-149">Například je možné selektivní přístup k databázi k získání informací o související entity bez načtení všechny vloženého DbContext.</span><span class="sxs-lookup"><span data-stu-id="da5c1-149">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="da5c1-150">V následujícím příkladu se používá získat počet příspěvků v blogu bez načtení příspěvky:</span><span class="sxs-lookup"><span data-stu-id="da5c1-150">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

``` csharp
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
<span data-ttu-id="da5c1-151">Všimněte si, že o této několik věcí:</span><span class="sxs-lookup"><span data-stu-id="da5c1-151">A few things to notice about this:</span></span>
* <span data-ttu-id="da5c1-152">Konstruktor je privátní, protože je vždy jen volá pomocí EF Core a existuje další veřejný konstruktor pro obecné použití.</span><span class="sxs-lookup"><span data-stu-id="da5c1-152">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="da5c1-153">Kód pomocí vloženého služby (to znamená, context) je obrany proti ho právě `null` zpracovat případy, ve kterém není EF Core vytvořit instanci.</span><span class="sxs-lookup"><span data-stu-id="da5c1-153">The code using the injected service (that is, the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="da5c1-154">Protože služby jsou uložena ve vlastnosti pro čtení a zápisu se resetuje při entity je připojený k nové instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="da5c1-154">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="da5c1-155">Vkládání uvolněn objekt DbContext, jako je to často považuje za proti vzor od jeho páry v odstupu vaše typy entit přímo na EF Core.</span><span class="sxs-lookup"><span data-stu-id="da5c1-155">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="da5c1-156">Pečlivě zvažte všechny možnosti před využitím vkládání služby následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="da5c1-156">Carefully consider all options before using service injection like this.</span></span>
