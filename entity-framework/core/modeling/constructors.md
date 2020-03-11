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
# <a name="entity-types-with-constructors"></a>Typy entit s konstruktory

> [!NOTE]  
> Tato funkce je v EF Core 2,1 novinkou.

Počínaje EF Core 2,1 je nyní možné definovat konstruktor s parametry a nechat EF Core volat tento konstruktor při vytváření instance entity. Parametry konstruktoru mohou být vázány na mapované vlastnosti nebo různé druhy služeb pro usnadnění chování, jako je opožděné načítání.

> [!NOTE]  
> Od EF Core 2,1 je všechny vazby konstruktoru podle konvence. Konfigurace konkrétních konstruktorů, které se mají použít, je plánována v budoucí verzi.

## <a name="binding-to-mapped-properties"></a>Vazba na mapované vlastnosti

Vezměte v úvahu typický model blogu/post:

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

Když EF Core vytváří instance těchto typů, například pro výsledky dotazu, nejprve vyvolá výchozí konstruktor bez parametrů a pak nastaví každou vlastnost na hodnotu z databáze. Pokud však EF Core najde parametrizovaný konstruktor s názvy parametrů a typy, které odpovídají hodnotám mapovaných vlastností, pak místo toho bude volat parametrizovaný konstruktor s hodnotami těchto vlastností a nebude explicitně nastavovat každou vlastnost. Příklad:

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

Pamatujte na tyto věci:

* Ne všechny vlastnosti musí mít parametry konstruktoru. Například vlastnost post. Content není nastavena žádným parametrem konstruktoru, takže EF Core nastaví je po volání konstruktoru normálním způsobem.
* Typy parametrů a názvy musí odpovídat typům a názvům vlastností, s tím rozdílem, že vlastnosti mohou být Pascal-použita, zatímco parametry jsou ve stylu CamelCase-použita.
* EF Core nemůže pomocí konstruktoru nastavit navigační vlastnosti (například blog nebo příspěvky výše).
* Konstruktor může být veřejný, privátní nebo má jakékoliv jiné přístupnost. Nicméně opožděné načítání proxy vyžaduje, aby byl konstruktor přístupný z třídy dědění proxy. Obvykle to znamená, že je buď veřejná, nebo chráněná.

### <a name="read-only-properties"></a>Vlastnosti jen pro čtení

Po nastavení vlastností prostřednictvím konstruktoru může mít smysl, aby bylo některé z nich jen pro čtení. EF Core to podporuje, ale existují několik věcí, které je třeba najít:

* Vlastnosti bez setter nejsou namapované podle konvence. (To znamená, že má za následek mapování vlastností, které by neměly být namapovány, například vypočítané vlastnosti.)
* Použití automaticky generovaných hodnot klíčů vyžaduje klíčovou vlastnost, která je pro čtení i zápis, protože při vkládání nových entit musí být klíčová hodnota nastavena pomocí generátoru klíčů.

Snadný způsob, jak se těmto akcím vyhnout, je použití privátních setter. Příklad:

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

EF Core vidí vlastnost s privátním setter jako pro čtení i zápis, což znamená, že všechny vlastnosti jsou namapované jako dřív a klíč může být vygenerovaný i nadále uložený.

Alternativou k použití privátních Setter je vytvořit vlastnosti, které jsou ve skutečnosti jen pro čtení, a přidat explicitní mapování v OnModelCreating. Podobně je možné některé vlastnosti zcela odebrat a nahradit je pouze poli. Zvažte například tyto typy entit:

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

A tato konfigurace v OnModelCreating:

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

Co je potřeba vzít v vědomí:

* Klíč "Property" je nyní pole. Nejedná se o `readonly` pole, aby bylo možné použít klíče generované úložištěm.
* Ostatní vlastnosti jsou vlastnosti jen pro čtení nastavené pouze v konstruktoru.
* Pokud je hodnota primárního klíče vždy nastavena pomocí EF nebo načtena z databáze, pak není nutné ji zahrnout do konstruktoru. Tím se klíč "Property" ponechá jako jednoduché pole a zruší se tak, že by neměl být nastaven explicitně při vytváření nových blogů nebo příspěvků.

> [!NOTE]  
> Výsledkem tohoto kódu bude upozornění kompilátoru "169", což značí, že pole není nikdy použito. To je možné ignorovat, protože ve realitě EF Core používá pole extralinguistic způsobem.

## <a name="injecting-services"></a>Vkládání služeb

EF Core může také vložit "služby" do konstruktoru typu entity. Například lze vložit následující:

* `DbContext` – aktuální kontext instance, který lze také zadat jako odvozený typ DbContext
* `ILazyLoader` – služba opožděného načítání – další podrobnosti najdete v [dokumentaci s opožděným načtením](../querying/related-data.md) .
* `Action<object, string>` – delegát opožděného načítání – další podrobnosti najdete v [dokumentaci s opožděným načtením](../querying/related-data.md) .
* `IEntityType` – metadata EF Core přidružená k tomuto typu entity

> [!NOTE]  
> Od EF Core 2,1 mohou být vloženy pouze služby známé EF Core. Podpora pro vkládání aplikačních služeb se považuje za budoucí verzi.

Například vložená DbContext může být použita k selektivnímu přístupu do databáze, aby získala informace o souvisejících entitách bez jejich načtení. V následujícím příkladu se používá k získání počtu příspěvků na blogu bez načtení příspěvků:

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

K tomu si můžete všimnout několika věcí:

* Konstruktor je privátní, protože je někdy volán pouze pomocí EF Core a existuje další veřejný konstruktor pro obecné použití.
* Kód, který používá vloženou službu (tj. kontext), je obrannou linií na `null` pro zpracování případů, kde EF Core nevytváří instanci.
* Vzhledem k tomu, že je služba uložena ve vlastnosti pro čtení a zápis, bude resetována, když je entita připojena k nové instanci kontextu.

> [!WARNING]  
> Vložení DbContext takto se často považuje za antipattern, protože Couples typy entit přímo do EF Core. Před použitím injektáže služby jako takového zvažte pečlivě všechny možnosti.
