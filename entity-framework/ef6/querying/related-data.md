---
title: Načítají se související entity – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: c359d8d32a88049213fd5e98e99fe49d7e3121a3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417086"
---
# <a name="loading-related-entities"></a>Načítají se související entity.

Entity Framework podporuje tři způsoby, jak načítat související Eager načítání dat, opožděné načítání a explicitní načítání. Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.

## <a name="eagerly-loading"></a>Eagerly načítání

Eager načítání je proces, při kterém dotaz pro jeden typ entity také načte související entity jako součást dotazu. Načítání Eager se dosahuje pomocí metody include. Například níže uvedené dotazy načtou Blogy a všechny příspěvky týkající se jednotlivých blogů.

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts.
    var blogs1 = context.Blogs
                        .Include(b => b.Posts)
                        .ToList();

    // Load one blog and its related posts.
    var blog1 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include(b => b.Posts)
                       .FirstOrDefault();

    // Load all blogs and related posts
    // using a string to specify the relationship.
    var blogs2 = context.Blogs
                        .Include("Posts")
                        .ToList();

    // Load one blog and its related posts
    // using a string to specify the relationship.
    var blog2 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include("Posts")
                       .FirstOrDefault();
}
```

> [!NOTE]
> Include je rozšiřující metoda v oboru názvů System. data. entity, takže se ujistěte, že používáte tento obor názvů.

### <a name="eagerly-loading-multiple-levels"></a>Eagerly načítání více úrovní

Je také možné eagerly načíst více úrovní souvisejících entit. Níže uvedené dotazy ukazují příklady toho, jak to udělat pro vlastnosti kolekce i navigační navigace.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments.
    var blogs1 = context.Blogs
                        .Include(b => b.Posts.Select(p => p.Comments))
                        .ToList();

    // Load all users, their related profiles, and related avatar.
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships.
    var blogs2 = context.Blogs
                        .Include("Posts.Comments")
                        .ToList();

    // Load all users, their related profiles, and related avatar  
    // using a string to specify the relationships.
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

> [!NOTE]
> V tuto chvíli není možné filtrovat, které související entity se načítají. Zahrnutí se vždycky přenese do všech souvisejících entit.  

## <a name="lazy-loading"></a>Opožděné načítání

Opožděné načítání je proces, při kterém je entita nebo kolekce entit automaticky načtena z databáze při prvním otevření vlastnosti odkazující na entitu nebo entity. Při použití typů entit POCO je dosaženo opožděné načítání vytvořením instancí odvozených typů proxy a následným přepsáním virtuálních vlastností pro přidání zavěšení zatížení. Pokud například používáte třídu entity blogu definovanou níže, související příspěvky se načtou při prvním otevření navigační vlastnosti příspěvky:

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }
    public string Tags { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

### <a name="turn-lazy-loading-off-for-serialization"></a>Zapnout opožděné načítání pro serializaci

Opožděné načítání a serializace se dobře nespojují. Pokud nemusíte pozor, můžete ukončit dotazování pro celou databázi, a to jenom v případě, že je povolené opožděné načítání. Většina serializátorů funguje při přístupu k jednotlivým vlastnostem instance typu. Přístup k vlastnostem aktivuje opožděné načítání, takže se další entity získají serializovat. K těmto vlastnostem entit se dostanete a načtou se i další entity. Před serializací entity je dobrým zvykem zapnout opožděné načítání. Následující části vysvětlují, jak to udělat.

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a>Vypnutí opožděného načítání pro konkrétní navigační vlastnosti

Opožděné načítání kolekce příspěvků lze vypnout tím, že vlastnost posts není virtuální:

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }
    public string Tags { get; set; }

    public ICollection<Post> Posts { get; set; }
}
```

Načítání kolekce příspěvků lze stále dosáhnout pomocí Eager načítání (viz *eagerly* Load výše) nebo metody Load (viz *explicitní načtení* níže).

### <a name="turn-off-lazy-loading-for-all-entities"></a>Vypnout opožděné načítání pro všechny entity

Opožděné načítání lze vypnout pro všechny entity v kontextu nastavením příznaku na vlastnost konfigurace. Příklad:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```

Načítání souvisejících entit se stále může dosáhnout pomocí Eager načítání (viz *eagerly* Load výše) nebo metodou Load (viz *explicitní načítání* níže).

## <a name="explicitly-loading"></a>Explicitní načítání

I v případě, že je opožděné načítání zakázané, je stále možné laxně vytvářená načíst související entity, ale je nutné provést explicitní volání. K tomu slouží metoda Load pro položku související entity. Příklad:

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post.
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string.
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog.
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog
    // using a string to specify the relationship.
    context.Entry(blog).Collection("Posts").Load();
}
```

> [!NOTE]
> Metoda reference by měla být použita, pokud má entita navigační vlastnost pro jinou jednu entitu. Na druhé straně by se měla použít metoda kolekce, pokud má entita navigační vlastnost pro kolekci jiných entit.

### <a name="applying-filters-when-explicitly-loading-related-entities"></a>Použití filtrů při explicitním načítání souvisejících entit

Metoda dotazu poskytuje přístup k podkladovým dotazům, které Entity Framework použijí při načítání souvisejících entit. Pak můžete použít LINQ k aplikování filtrů na dotaz před jeho provedením voláním metody rozšíření LINQ, jako je ToList –, Load atd. Metoda dotazu může být použita s oběma vlastnostmi odkazu i navigace v kolekci, ale je nejužitečnější pro kolekce, kde je lze použít pro načtení pouze části kolekce. Příklad:

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog.
    context.Entry(blog)
           .Collection(b => b.Posts)
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog
    // using a string to specify the relationship.
    context.Entry(blog)
           .Collection("Posts")
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();
}
```

Při použití metody dotazu obvykle je nejlepší vypnout opožděné načítání pro navigační vlastnost. Důvodem je, že v opačném případě může být celá kolekce automaticky načtena mechanismem opožděného načítání buď před, nebo po provedení filtrovaného dotazu.

> [!NOTE]
> Zatímco relaci lze zadat jako řetězec namísto lambda výrazu, vrácený parametr IQueryable není obecný, pokud je použit řetězec, a proto je metoda přetypování obvykle potřebná předtím, než je možné s ní provádět cokoli užitečné.

## <a name="using-query-to-count-related-entities-without-loading-them"></a>Použití dotazu k počítání souvisejících entit bez jejich načtení

Někdy je užitečné zjistit, kolik entit souvisí s jinou entitou v databázi, aniž by to mělo za následek nasazování všech těchto entit. K tomu lze použít metodu dotazu s metodou LINQ Count. Příklad:

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has.
    var postCount = context.Entry(blog)
                           .Collection(b => b.Posts)
                           .Query()
                           .Count();
}
```
