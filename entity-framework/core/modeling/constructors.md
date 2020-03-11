---
title: Typy entit s konstruktory – EF Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: ddfaa8eebde388a9d3309f21b8891de593077956
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417327"
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="819c9-102">Typy entit s konstruktory</span><span class="sxs-lookup"><span data-stu-id="819c9-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="819c9-103">Tato funkce je v EF Core 2,1 novinkou.</span><span class="sxs-lookup"><span data-stu-id="819c9-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="819c9-104">Počínaje EF Core 2,1 je nyní možné definovat konstruktor s parametry a nechat EF Core volat tento konstruktor při vytváření instance entity.</span><span class="sxs-lookup"><span data-stu-id="819c9-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="819c9-105">Parametry konstruktoru mohou být vázány na mapované vlastnosti nebo různé druhy služeb pro usnadnění chování, jako je opožděné načítání.</span><span class="sxs-lookup"><span data-stu-id="819c9-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="819c9-106">Od EF Core 2,1 je všechny vazby konstruktoru podle konvence.</span><span class="sxs-lookup"><span data-stu-id="819c9-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="819c9-107">Konfigurace konkrétních konstruktorů, které se mají použít, je plánována v budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="819c9-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="819c9-108">Vazba na mapované vlastnosti</span><span class="sxs-lookup"><span data-stu-id="819c9-108">Binding to mapped properties</span></span>

<span data-ttu-id="819c9-109">Vezměte v úvahu typický model blogu/post:</span><span class="sxs-lookup"><span data-stu-id="819c9-109">Consider a typical Blog/Post model:</span></span>

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

<span data-ttu-id="819c9-110">Když EF Core vytváří instance těchto typů, například pro výsledky dotazu, nejprve vyvolá výchozí konstruktor bez parametrů a pak nastaví každou vlastnost na hodnotu z databáze.</span><span class="sxs-lookup"><span data-stu-id="819c9-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="819c9-111">Pokud však EF Core najde parametrizovaný konstruktor s názvy parametrů a typy, které odpovídají hodnotám mapovaných vlastností, pak místo toho bude volat parametrizovaný konstruktor s hodnotami těchto vlastností a nebude explicitně nastavovat každou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="819c9-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="819c9-112">Příklad:</span><span class="sxs-lookup"><span data-stu-id="819c9-112">For example:</span></span>

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

<span data-ttu-id="819c9-113">Pamatujte na tyto věci:</span><span class="sxs-lookup"><span data-stu-id="819c9-113">Some things to note:</span></span>

* <span data-ttu-id="819c9-114">Ne všechny vlastnosti musí mít parametry konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="819c9-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="819c9-115">Například vlastnost post. Content není nastavena žádným parametrem konstruktoru, takže EF Core nastaví je po volání konstruktoru normálním způsobem.</span><span class="sxs-lookup"><span data-stu-id="819c9-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="819c9-116">Typy parametrů a názvy musí odpovídat typům a názvům vlastností, s tím rozdílem, že vlastnosti mohou být Pascal-použita, zatímco parametry jsou ve stylu CamelCase-použita.</span><span class="sxs-lookup"><span data-stu-id="819c9-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="819c9-117">EF Core nemůže pomocí konstruktoru nastavit navigační vlastnosti (například blog nebo příspěvky výše).</span><span class="sxs-lookup"><span data-stu-id="819c9-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="819c9-118">Konstruktor může být veřejný, privátní nebo má jakékoliv jiné přístupnost.</span><span class="sxs-lookup"><span data-stu-id="819c9-118">The constructor can be public, private, or have any other accessibility.</span></span> <span data-ttu-id="819c9-119">Nicméně opožděné načítání proxy vyžaduje, aby byl konstruktor přístupný z třídy dědění proxy.</span><span class="sxs-lookup"><span data-stu-id="819c9-119">However, lazy-loading proxies require that the constructor is accessible from the inheriting proxy class.</span></span> <span data-ttu-id="819c9-120">Obvykle to znamená, že je buď veřejná, nebo chráněná.</span><span class="sxs-lookup"><span data-stu-id="819c9-120">Usually this means making it either public or protected.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="819c9-121">Vlastnosti jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="819c9-121">Read-only properties</span></span>

<span data-ttu-id="819c9-122">Po nastavení vlastností prostřednictvím konstruktoru může mít smysl, aby bylo některé z nich jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="819c9-122">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="819c9-123">EF Core to podporuje, ale existují několik věcí, které je třeba najít:</span><span class="sxs-lookup"><span data-stu-id="819c9-123">EF Core supports this, but there are some things to look out for:</span></span>

* <span data-ttu-id="819c9-124">Vlastnosti bez setter nejsou namapované podle konvence.</span><span class="sxs-lookup"><span data-stu-id="819c9-124">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="819c9-125">(To znamená, že má za následek mapování vlastností, které by neměly být namapovány, například vypočítané vlastnosti.)</span><span class="sxs-lookup"><span data-stu-id="819c9-125">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="819c9-126">Použití automaticky generovaných hodnot klíčů vyžaduje klíčovou vlastnost, která je pro čtení i zápis, protože při vkládání nových entit musí být klíčová hodnota nastavena pomocí generátoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="819c9-126">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="819c9-127">Snadný způsob, jak se těmto akcím vyhnout, je použití privátních setter.</span><span class="sxs-lookup"><span data-stu-id="819c9-127">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="819c9-128">Příklad:</span><span class="sxs-lookup"><span data-stu-id="819c9-128">For example:</span></span>

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

<span data-ttu-id="819c9-129">EF Core vidí vlastnost s privátním setter jako pro čtení i zápis, což znamená, že všechny vlastnosti jsou namapované jako dřív a klíč může být vygenerovaný i nadále uložený.</span><span class="sxs-lookup"><span data-stu-id="819c9-129">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="819c9-130">Alternativou k použití privátních Setter je vytvořit vlastnosti, které jsou ve skutečnosti jen pro čtení, a přidat explicitní mapování v OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="819c9-130">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="819c9-131">Podobně je možné některé vlastnosti zcela odebrat a nahradit je pouze poli.</span><span class="sxs-lookup"><span data-stu-id="819c9-131">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="819c9-132">Zvažte například tyto typy entit:</span><span class="sxs-lookup"><span data-stu-id="819c9-132">For example, consider these entity types:</span></span>

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

<span data-ttu-id="819c9-133">A tato konfigurace v OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="819c9-133">And this configuration in OnModelCreating:</span></span>

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

<span data-ttu-id="819c9-134">Co je potřeba vzít v vědomí:</span><span class="sxs-lookup"><span data-stu-id="819c9-134">Things to note:</span></span>

* <span data-ttu-id="819c9-135">Klíč "Property" je nyní pole.</span><span class="sxs-lookup"><span data-stu-id="819c9-135">The key "property" is now a field.</span></span> <span data-ttu-id="819c9-136">Nejedná se o `readonly` pole, aby bylo možné použít klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="819c9-136">It is not a `readonly` field so that store-generated keys can be used.</span></span>
* <span data-ttu-id="819c9-137">Ostatní vlastnosti jsou vlastnosti jen pro čtení nastavené pouze v konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="819c9-137">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="819c9-138">Pokud je hodnota primárního klíče vždy nastavena pomocí EF nebo načtena z databáze, pak není nutné ji zahrnout do konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="819c9-138">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="819c9-139">Tím se klíč "Property" ponechá jako jednoduché pole a zruší se tak, že by neměl být nastaven explicitně při vytváření nových blogů nebo příspěvků.</span><span class="sxs-lookup"><span data-stu-id="819c9-139">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="819c9-140">Výsledkem tohoto kódu bude upozornění kompilátoru "169", což značí, že pole není nikdy použito.</span><span class="sxs-lookup"><span data-stu-id="819c9-140">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="819c9-141">To je možné ignorovat, protože ve realitě EF Core používá pole extralinguistic způsobem.</span><span class="sxs-lookup"><span data-stu-id="819c9-141">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="819c9-142">Vkládání služeb</span><span class="sxs-lookup"><span data-stu-id="819c9-142">Injecting services</span></span>

<span data-ttu-id="819c9-143">EF Core může také vložit "služby" do konstruktoru typu entity.</span><span class="sxs-lookup"><span data-stu-id="819c9-143">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="819c9-144">Například lze vložit následující:</span><span class="sxs-lookup"><span data-stu-id="819c9-144">For example, the following can be injected:</span></span>

* <span data-ttu-id="819c9-145">`DbContext` – aktuální kontext instance, který lze také zadat jako odvozený typ DbContext</span><span class="sxs-lookup"><span data-stu-id="819c9-145">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="819c9-146">`ILazyLoader` – služba opožděného načítání – další podrobnosti najdete v [dokumentaci s opožděným načtením](../querying/related-data.md) .</span><span class="sxs-lookup"><span data-stu-id="819c9-146">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="819c9-147">`Action<object, string>` – delegát opožděného načítání – další podrobnosti najdete v [dokumentaci s opožděným načtením](../querying/related-data.md) .</span><span class="sxs-lookup"><span data-stu-id="819c9-147">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="819c9-148">`IEntityType` – metadata EF Core přidružená k tomuto typu entity</span><span class="sxs-lookup"><span data-stu-id="819c9-148">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="819c9-149">Od EF Core 2,1 mohou být vloženy pouze služby známé EF Core.</span><span class="sxs-lookup"><span data-stu-id="819c9-149">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="819c9-150">Podpora pro vkládání aplikačních služeb se považuje za budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="819c9-150">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="819c9-151">Například vložená DbContext může být použita k selektivnímu přístupu do databáze, aby získala informace o souvisejících entitách bez jejich načtení.</span><span class="sxs-lookup"><span data-stu-id="819c9-151">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="819c9-152">V následujícím příkladu se používá k získání počtu příspěvků na blogu bez načtení příspěvků:</span><span class="sxs-lookup"><span data-stu-id="819c9-152">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

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

<span data-ttu-id="819c9-153">K tomu si můžete všimnout několika věcí:</span><span class="sxs-lookup"><span data-stu-id="819c9-153">A few things to notice about this:</span></span>

* <span data-ttu-id="819c9-154">Konstruktor je privátní, protože je někdy volán pouze pomocí EF Core a existuje další veřejný konstruktor pro obecné použití.</span><span class="sxs-lookup"><span data-stu-id="819c9-154">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="819c9-155">Kód, který používá vloženou službu (tj. kontext), je obrannou linií na `null` pro zpracování případů, kde EF Core nevytváří instanci.</span><span class="sxs-lookup"><span data-stu-id="819c9-155">The code using the injected service (that is, the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="819c9-156">Vzhledem k tomu, že je služba uložena ve vlastnosti pro čtení a zápis, bude resetována, když je entita připojena k nové instanci kontextu.</span><span class="sxs-lookup"><span data-stu-id="819c9-156">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="819c9-157">Vložení DbContext takto se často považuje za antipattern, protože Couples typy entit přímo do EF Core.</span><span class="sxs-lookup"><span data-stu-id="819c9-157">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="819c9-158">Před použitím injektáže služby jako takového zvažte pečlivě všechny možnosti.</span><span class="sxs-lookup"><span data-stu-id="819c9-158">Carefully consider all options before using service injection like this.</span></span>
