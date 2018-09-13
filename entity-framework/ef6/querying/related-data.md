---
title: Načítají se související entity - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: 1d59e6c079e306158ed918cde16e69c9cb084711
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489047"
---
# <a name="loading-related-entities"></a>Načítají se související entity
Entity Framework podporuje tři způsoby, jak načíst související data - eager načítání, opožděné načtení a explicitní načtení. Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.  

## <a name="eagerly-loading"></a>Například načítání  

Předběžné načítání je proces, kterým dotazu pro jeden typ entity se také načtou související entity jako součást dotazu. Předběžné načítání je dosaženo pomocí metody Include. Například níže uvedené dotazy se načte blogové příspěvky a všechny příspěvky vztahující se k blogů.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                          .Include(b => b.Posts)
                          .ToList();

    // Load one blogs and its related posts
    var blog1 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include(b => b.Posts)
                        .FirstOrDefault();

    // Load all blogs and related posts  
    // using a string to specify the relationship
    var blogs2 = context.Blogs
                          .Include("Posts")
                          .ToList();

    // Load one blog and its related posts  
    // using a string to specify the relationship
    var blog2 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include("Posts")
                        .FirstOrDefault();
}
```  

Všimněte si, zahrnout je rozšiřující metodu v oboru názvů System.Data.Entity proto se ujistěte, že používáte tento obor názvů.  

### <a name="eagerly-loading-multiple-levels"></a>Například načítání více úrovní  

Je také možné například načíst několik úrovní související entity. Níže uvedené dotazy ukazují příklady toho, jak to provést u kolekce a odkaz na navigační vlastnosti.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                       .Include(b => b.Posts.Select(p => p.Comments))
                       .ToList();

    // Load all users their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                       .Include("Posts.Comments")
                       .ToList();

    // Load all users their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

Všimněte si, že není aktuálně můžete provést filtrování souvisejících entit, které jsou načteny. Zahrnout bude vždy umožňuje přinést si všechny související entity.  

## <a name="lazy-loading"></a>Opožděné načtení  

Opožděné načtení je proces, kterým entitu nebo kolekci entit se automaticky načtou z databáze při prvním přístupu k vlastnosti odkazující na entity nebo entities. Při použití typů entit POCO, opožděné načtení se dosahuje prostřednictvím vytváření instancí typů odvozených proxy serveru a potom přepsáním virtuální vlastnosti přidáte hook načítání. Například pokud používáte třídu entity blogu definovaná níže, souvisejících příspěvků se načtou při prvním příspěvky vlastnost navigace pracuje:  

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

### <a name="turn-lazy-loading-off-for-serialization"></a>Zapnout opožděné načtení vypnout pro serializaci  

Serializace a opožděné načtení Nekombinujte dobře a nevyloučíte můžete skončit dotazování pro celou databázi pouze z důvodu opožděné načtení je povolená. Většina serializátory práci díky přístupu do každou vlastnost v instanci typu. Přístup k vlastnostem aktivuje opožděné načtení, tak se serializují dalších entit. S těmito entitami přístup k vlastnostem a ještě více entit se načtou. Je vhodné zapnout opožděné načtení vypnout před serializovat entity. Následující části vysvětlují, jak to udělat.  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a>Vypnutí opožděné načtení pro konkrétní navigační vlastnosti  

Opožděné načtení kolekce příspěvky můžete vypnout tak, že vlastnost příspěvky nevirtuální:  

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

Načítání příspěvků kolekce stále lze dosáhnout pomocí předběžné načítání (naleznete v tématu *například načítání* výše) nebo metodu načtení (naleznete v tématu *explicitně načítání* níže).  

### <a name="turn-off-lazy-loading-for-all-entities"></a>Vypnout opožděné načtení pro všechny entity  

Opožděné načtení může být vypnuté pro všechny entity v kontextu nastavením příznaku v konfigurační vlastnosti. Příklad:  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

Načítání souvisejících entit stále lze dosáhnout pomocí předběžné načítání (naleznete v tématu *například načítání* výše) nebo metodu načtení (naleznete v tématu *explicitně načítání* níže).  

## <a name="explicitly-loading"></a>Explicitně načítání  

I přes opožděné načítání je zakázáno je stále možné laxně načtení souvisejících entit, ale je nutné provést pomocí explicitní volání konstruktoru. K tomu použít metodu zatížení u související entity položky. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string  
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog  
    // using a string to specify the relationship
    context.Entry(blog).Collection("Posts").Load();
}
```  

Všimněte si, že metodu odkaz by měl být pokud má entita navigační vlastnost pro další jednu entitu. Metodu kolekce na druhé straně by měl použít, pokud má entita navigační vlastnost kolekce jiných entit.  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a>Použití filtrů při načítání explicitně související entity  

Metoda dotazu poskytuje přístup k základní dotaz, který Entity Framework bude používat při načítání souvisejících entit. Potom můžete LINQ a nastavte filtry pro dotaz před spuštěním pomocí volání metody rozšíření LINQ jako je například ToList zatížení, atd. Metodu dotazu je možné použít s vlastnostmi navigační odkaz a kolekce, ale je zvláště užitečná pro kolekce, ve kterém je možné načíst pouze část shromažďování. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
        .Collection("Posts")
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  

Při použití metody dotazu je obvykle nejlepší volbou, chcete-li vypnout opožděné načtení pro navigační vlastnost. Je to proto jinak celou kolekci mohou získat načteny automaticky opožděné načtení mechanismem před nebo po provedení filtrovaného dotazu.  

Všimněte si, že během relace můžete zadat jako řetězec namísto výrazu lambda, vrácený typ IQueryable není obecná při řetězec se používá, a proto metodu přetypování je obvykle potřeba před nic užitečného můžete s ním dá dělat.  

## <a name="using-query-to-count-related-entities-without-loading-them"></a>Pomocí dotazu na počet souvisejících entit bez jejich načtení  

Někdy je užitečné vědět, kolik entity se vztahují na jinou entitu v databázi bez ve skutečnosti by tím narůstaly náklady na načtení těchto entit. K tomu je možné metodu dotazu pomocí LINQ Count – metoda. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has  
    var postCount = context.Entry(blog)
                          .Collection(b => b.Posts)
                          .Query()
                          .Count();
}
```  
