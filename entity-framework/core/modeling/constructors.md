---
title: "Typy entit pomocí konstruktorů - EF jádra"
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 2632488569c538a11c7a31a9a866d2fadb29eeb5
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="entity-types-with-constructors"></a>Typy entit pomocí konstruktorů

> [!NOTE]  
> Tato funkce je nového v EF základní 2.1.

Od verze 2.1 základní EF, je nyní možné určit konstruktor s parametry a základní EF volání tento konstruktor při vytváření instance entity. Konstruktor parametry mohou být vázány na namapované vlastnosti, nebo na různé typy služeb pro usnadnění jako chování opožděného načítání.

> [!NOTE]  
> Od verze 2.1 základní EF všechny konstruktor vazba je pomocí konvence. Konfigurace konkrétní konstruktory používat je plánovaná pro budoucí použití.

## <a name="binding-to-mapped-properties"></a>Vytvoření vazby na namapované vlastnosti

Vezměte v úvahu typické modelu příspěvku na blogu:

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

Když EF základní vytváří instance tyto typy, jako výsledků dotazu, bude nejprve volat výchozí konstruktor bez parametrů a pak každou vlastnost nastavena na hodnotu z databáze. Ale pokud EF základní vyhledá parametrizované konstruktor s názvy parametrů a typy, které se shodují mapovat vlastnosti a potom ho bude místo toho volat parametrizované konstruktor s hodnotami pro tyto vlastnosti a nebude explicitně nastaven každou vlastnost. Příklad:

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
Některé věci Všimněte si:
* Ne všechny vlastnosti musí být parametry konstruktor. Například vlastnost Post.Content není nastaven libovolný parametr konstruktoru, takže EF základní ho nastavit po volání metody konstruktoru běžným způsobem.
* Typy parametrů a názvy shodovat typy vlastností a názvy, s tím rozdílem, že vlastnosti může být použita Pascal parametry jsou ve formátu camelCase.
* Základní EF nelze nastavit vlastnosti navigace (například blogu nebo příspěvky výše) pomocí konstruktoru.
* Konstruktor může být veřejné, privátní, nebo jiné usnadnění.

### <a name="read-only-properties"></a>Vlastnosti jen pro čtení

Jakmile se nastaveny vlastnosti prostřednictvím konstruktoru ho mít smysl Chcete-li některá z nich jen pro čtení. Základní EF to podporuje, ale některé kroky, chcete-li o:
* Vlastnosti bez mechanismy získání nejsou mapovat pomocí konvence. (Díky tomu se obvykle mapy – vlastnosti, které nelze mapovat, jako je například počítané vlastnosti.)
* Použití automaticky generované hodnoty klíče vyžaduje klíčové vlastnosti, která je pro čtení a zápis, protože hodnota klíče je třeba nastavit klíče generátorem při vkládání nové entity.

Snadný způsob, jak se vyhnout tyto akce se má používat privátní setter. Příklad:
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
Základní EF uvidí vlastnost s privátní setter jako pro čtení a zápis, což znamená, že jsou jako předtím namapované všechny vlastnosti a klíč můžete stále být generovaný úložištěm.

Alternativu k použití privátního setter je nastavení vlastnosti skutečně jen pro čtení a přidání více explicitní mapování v OnModelCreating. Některé vlastnosti, můžete úplně odstraněny a nahradí pouze pole. Představte si třeba tyto typy entit:

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
Co je potřeba Všimněte si:
* Pole je nyní klíč "vlastnost". Není `readonly` pole tak, aby bylo možné použít klíče generované úložištěm. 
* Ostatní vlastnosti jsou jen pro čtení vlastnosti nastavit pouze v konstruktoru.
* Pokud hodnotu primárního klíče je vždy jen nastaven EF ani načíst z databáze, je potřeba zahrnout do konstruktoru. Tím zůstane klíč "vlastnost" jako jednoduchý pole a jasně ukazuje, že ho by neměla být explicitně nastaveno při vytváření nové blogy nebo příspěvky.

> [!NOTE]  
> Tento kód bude mít za následek kompilátoru upozornění '169' indikující, že pole se nikdy nepoužívá. To můžete ignorovat, protože ve skutečnosti EF základní používá pole extralinguistic způsobem.

## <a name="injecting-services"></a>Vložení služby

Základní EF můžete také vložit "služby" do konstruktoru typu entity. Například následující může vložit:
* `DbContext` -aktuální instance kontextu, které mohou být zadány jako vaše odvozený typ DbContext
* `ILazyLoader` -Služba opožděného načítání--najdete v článku [opožděného načítání dokumentaci](../querying/related-data.md) další podrobnosti
* `Action<object, string>` -opožděného načítání delegáta – najdete v článku [opožděného načítání dokumentaci](../querying/related-data.md) další podrobnosti
* `IEntityType` -metadata EF základní přidružené tomuto typu entity

> [!NOTE]  
> Od verze 2.1 základní EF může vložit pouze služby, známého EF jádra. Podpora pro vložení aplikační služby se považuje za pro budoucí použití.

Například vloženého DbContext slouží pro selektivní přístup k databázi chcete získat informace o entit v relaci bez načítání je všechny. V následujícím příkladu se používá získat počet příspěvky v blogu bez načítání v příspěvcích:

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
Všimněte si o tom několik akcí:
* Konstruktor je soukromé, protože je jen někdy pro volání EF jádra a je jiný veřejný konstruktor pro obecné použití.
* Kód pomocí služby vloženého (tj. v kontextu) je Obranným u ní se `null` pro zpracování případů, kde není EF základní vytvořit instanci.
* Protože služba je uložené v vlastnost pro čtení a zápis se resetuje po připojení k nové instance kontextu entity.

> [!WARNING]  
> Vložení DbContext takto často považuje za proti vzor vzhledem k tomu, že ho páry v odstupu vaší typy entit přímo na EF jádra. Pečlivě zvažte všechny možnosti před použitím služby vkládání takto.
