---
title: Typy entit s konstruktory – EF Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: 0536393d074d82583f47faae13cc22498193cb7e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994890"
---
# <a name="entity-types-with-constructors"></a>Typy entit s konstruktory

> [!NOTE]  
> Tato funkce je nového v EF Core 2.1.

Od verze EF Core 2.1, je teď možné definovat konstruktor s parametry a EF Core volání tohoto konstruktoru při vytváření instance entity. Parametry konstruktoru mohou být vázány na připojené vlastnosti nebo na různé typy služeb, aby se usnadnilo chování jako opožděné načtení.

> [!NOTE]  
> EF Core 2.1 všechny vazby konstruktor je podle konvence. Konfigurace specifické konstruktory používat pro budoucí verzi plánujeme přidat.

## <a name="binding-to-mapped-properties"></a>Vazba pro mapovanou vlastnosti

Zvažte typické modelu/blogu:

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

Když EF Core vytváří instance typů, například pro výsledky dotazu, bude nejprve volat výchozí konstruktor a nastavte jednotlivé vlastnosti na hodnotu z databáze. Ale pokud najde konstruktorem s EF Core mapovat názvy parametrů a typy, které odpovídají vlastnosti a pak bude místo toho volat Parametrizovaný konstruktor s hodnotami pro tyto vlastnosti a není explicitně nastavena každou vlastnost. Příklad:

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
Poznamenat několik věcí:
* Ne všechny vlastnosti musí mít parametry konstruktoru. Například vlastnost Post.Content není nastavena podle jakékoli parametr konstruktoru, takže EF Core ho nastavte po volání konstruktoru běžným způsobem.
* Typy parametrů a názvů musí odpovídat názvy a typy vlastností s tím rozdílem, že vlastnosti mohou být – jazyka Pascal – parametry jsou – ve formátu camelCase.
* EF Core nelze nastavit navigační vlastnosti (například blogu nebo příspěvky výše) pomocí konstruktoru.
* Konstruktor může být veřejný, privátní, nebo máte další usnadnění přístupu.

### <a name="read-only-properties"></a>Vlastnosti jen pro čtení

Jakmile se nastavují vlastnosti prostřednictvím konstruktoru může být vhodné provést některé z nich jen pro čtení. EF Core podporuje, ale k vyhledání navýšení kapacity na několik věcí:
* Vlastnosti bez setter nejsou mapovat pomocí konvence. (To se obvykle mapy – vlastnosti, které by neměly být mapována, například vypočítané vlastnosti.)
* Použití hodnoty automaticky generovaného klíče vyžaduje klíčová vlastnost, která je pro čtení i zápis, protože hodnota klíče, která se musí nastavit generátorem klíče při vložení nových entit.

Snadný způsob, jak se vyhnout tyto věci se má používat soukromé funkce setter. Příklad:
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
EF Core považuje čtení i zápis, což znamená, že všechny vlastnosti jsou mapovány stejně jako předtím a klíč se můžete stále možné generovaných úložištěm vlastnost s privátní metodu setter.

Alternativou k používání soukromý setter je nastavení vlastnosti skutečně jen pro čtení a přidat více explicitního mapování v OnModelCreating. Některé vlastnosti, můžete k úplnému odebrání a nahradí pouze pole. Představte si třeba tyto typy entit:

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
A tato konfigurace v OnModelCreating:
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
Co je potřeba mějte na paměti:
* Pole je nyní klíč "vlastnosti". Nejedná se `readonly` pole tak, aby klíče generované úložištěm můžete použít.
* Další vlastnosti jsou jen pro čtení vlastnosti nastavené jenom v konstruktoru.
* Pokud hodnotu primárního klíče je vždy jen nastavit EF nebo čtení z databáze, je potřeba zahrnout do konstruktoru. To opustí klíč "vlastnosti" jako jednoduchý pole a je zřejmé, že ho by neměla být explicitně nastaveno při vytváření nových blogů a příspěvky.

> [!NOTE]  
> Tento kód způsobí kompilátor varování "169" označující, že pole se nikdy nepoužívá. To můžete ignorovat, protože ve skutečnosti je EF Core pomocí pole extralinguistic způsobem.

## <a name="injecting-services"></a>Vkládání služeb

EF Core můžete také vložit "služby" do konstruktoru typu entity. Například následující může být vloženy:
* `DbContext` -aktuální instance kontextu, které mohou být zadány jako odvozený typ DbContext
* `ILazyLoader` -opožděné načtení služby – najdete v článku [opožděné načtení dokumentaci](../querying/related-data.md) další podrobnosti
* `Action<object, string>` -opožděné načtení delegáta--najdete v článku [opožděné načtení dokumentaci](../querying/related-data.md) další podrobnosti
* `IEntityType` -EF Core metadata přidružená k tomuto typu entity

> [!NOTE]  
> Od verze EF Core 2.1 lze vloženy pouze služeb, které EF Core. Podpora pro vkládání aplikace služby se považují za pro budoucí verzi.

Například je možné selektivní přístup k databázi k získání informací o související entity bez načtení všechny vloženého DbContext. V následujícím příkladu se používá získat počet příspěvků v blogu bez načtení příspěvky:

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
Všimněte si, že o této několik věcí:
* Konstruktor je privátní, protože je vždy jen volá pomocí EF Core a existuje další veřejný konstruktor pro obecné použití.
* Kód pomocí vloženého služby (to znamená, context) je obrany proti ho právě `null` zpracovat případy, ve kterém není EF Core vytvořit instanci.
* Protože služby jsou uložena ve vlastnosti pro čtení a zápisu se resetuje při entity je připojený k nové instance kontextu.

> [!WARNING]  
> Vkládání uvolněn objekt DbContext, jako je to často považuje za proti vzor od jeho páry v odstupu vaše typy entit přímo na EF Core. Pečlivě zvažte všechny možnosti před využitím vkládání služby následujícím způsobem.
