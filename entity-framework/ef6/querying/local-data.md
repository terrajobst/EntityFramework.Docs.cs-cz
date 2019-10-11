---
title: Místní data – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: efd646348d8a18bbeed2d0a0e708d4d36eb26eac
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182433"
---
# <a name="local-data"></a>Místní data
Spuštění dotazu LINQ přímo proti Negenerickými vždy pošle dotaz do databáze, ale přístup k datům, která jsou aktuálně v paměti, můžete získat pomocí vlastnosti Negenerickými. Local. Přístup k základnímu EF informací získáte také tak, že sledujete své entity pomocí metod DbContext. entry a DbContext. ChangeTracker. Entries. Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.  

## <a name="using-local-to-look-at-local-data"></a>Použití místní k zobrazení místních dat  

Místní vlastnost Negenerickými poskytuje jednoduchý přístup k entitám sady, které jsou aktuálně sledovány pomocí kontextu a nebyly označeny jako odstraněné. Přístup k místní vlastnosti nikdy nevede k odeslání dotazu do databáze. To znamená, že se obvykle používá poté, co byl dotaz již proveden. Metodu Load Extension lze použít k provedení dotazu, aby kontext sleduje výsledky. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs from the database into the context
    context.Blogs.Load();

    // Add a new blog to the context
    context.Blogs.Add(new Blog { Name = "My New Blog" });

    // Mark one of the existing blogs as Deleted
    context.Blogs.Remove(context.Blogs.Find(1));

    // Loop over the blogs in the context.
    Console.WriteLine("In Local: ");
    foreach (var blog in context.Blogs.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }

    // Perform a query against the database.
    Console.WriteLine("\nIn DbSet query: ");
    foreach (var blog in context.Blogs)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }
}
```  

Pokud máme dva Blogy v databázi-' ADO.NET blog ' s BlogIdem 1 a blogem sady Visual Studio s BlogIdem 2 – můžeme očekávat následující výstup:  

```console
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

To ukazuje tři body:  

- Nový blog "můj nový blog" je součástí místní kolekce, i když ještě nebyl uložen do databáze. Tento blog má primární klíč nula, protože databáze ještě negenerovala skutečný klíč pro entitu.  
- Blog ADO.NET není zahrnutý v místní kolekci, i když je stále sledován pomocí kontextu. Důvodem je to, že jsme ho odebrali z Negenerickými a tím ho označíte jako odstraněný.  
- Když se Negenerickými používá k provedení dotazu, je blog označený k odstranění (blog ADO.NET), který je součástí výsledků a nový blog (můj nový blog), který ještě nebyl uložen do databáze, není zahrnutý ve výsledcích. Důvodem je to, že Negenerickými provádí dotaz na databázi a vrácené výsledky vždycky odrážejí, co se nachází v databázi.  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a>Použití místní k přidání a odebrání entit z kontextu  

Místní vlastnost v Negenerickými vrací [kolekci ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) s událostmi, které jsou zaelné tak, aby zůstaly synchronizované s obsahem kontextu. To znamená, že entity je možné přidat nebo odebrat buď z místní kolekce, nebo z Negenerickými. Také to znamená, že dotazy, které přinášejí nové entity do kontextu, budou mít za následek aktualizaci místní kolekce těmito entitami. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework")).Load();  

    // Get the local collection and make some changes to it
    var localPosts = context.Posts.Local;
    localPosts.Add(new Post { Name = "What's New in EF" });
    localPosts.Remove(context.Posts.Find(1));  

    // Loop over the posts in the context.
    Console.WriteLine("In Local after entity-framework query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }

    var post1 = context.Posts.Find(1);
    Console.WriteLine(
        "State of post 1: {0} is {1}",
        post1.Name,  
        context.Entry(post1).State);  

    // Query some more posts from the database
    context.Posts.Where(p => p.Tags.Contains("asp.net").Load();  

    // Loop over the posts in the context again.
    Console.WriteLine("\nIn Local after asp.net query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }
}
```  

Za předpokladu, že máme několik příspěvků s příznakem "Entity-Framework" a "asp.net", může výstup vypadat přibližně takto:  

```console
In Local after entity-framework query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
State of post 1: EF Beginners Guide is Deleted

In Local after asp.net query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
Found 4: ASP.NET Beginners Guide with state Unchanged
```  

To ukazuje tři body:  

- Nový příspěvek "Co je nového v EF", který byl přidán do místní kolekce, se bude sledovat v kontextu ve stavu přidáno. V případě volání metody SaveChanges bude proto vložena do databáze.  
- Příspěvek, který byl odebrán z místní kolekce (Příručka pro začátečníky), je teď v kontextu označený jako odstraněný. Z databáze proto bude při volání metody SaveChanges odstraněna.  
- Další příspěvek (Příručka pro ASP.NET začátečník) načtený do kontextu s druhým dotazem se automaticky přidá do místní kolekce.  

K tomu, abyste si poznamenali místní informace, je to, že vzhledem k tomu, že se jedná o kolekci ObservableCollection výkon, není Skvělé pro velký počet entit. Pokud tedy v kontextu pracujete s tisíci entit, nemusí být vhodné použít místní.  

## <a name="using-local-for-wpf-data-binding"></a>Použití místní pro datovou vazbu WPF  

Místní vlastnost na Negenerickými lze použít přímo pro datovou vazbu v aplikaci WPF, protože se jedná o instanci kolekci ObservableCollection. Jak je popsáno v předchozích částech to znamená, že se automaticky synchronizuje s obsahem kontextu a obsah tohoto kontextu bude automaticky zůstat synchronizovaný s ním. Všimněte si, že je nutné předem naplnit místní kolekci daty, aby bylo možné navazovat vazby, protože místní nikdy nezpůsobí databázový dotaz.  

Nejedná se o vhodné místo pro úplnou ukázku datových vazeb WPF, ale klíčové prvky jsou:  

- Nastavení zdroje vazby  
- Navažte vazby na místní vlastnost vaší sady.  
- Naplňte místní pomocí dotazu do databáze.  

## <a name="wpf-binding-to-navigation-properties"></a>Vazba WPF na navigační vlastnosti  

Pokud provádíte datovou vazbu hlavního/podrobného data, můžete chtít svázat zobrazení podrobností s vlastností navigace jedné z entit. Snadný způsob, jak tuto práci udělat, je použít pro navigační vlastnost kolekci ObservableCollection. Příklad:  

``` csharp
public class Blog
{
    private readonly ObservableCollection<Post> _posts =
        new ObservableCollection<Post>();

    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual ObservableCollection<Post> Posts
    {
        get { return _posts; }
    }
}
```  

## <a name="using-local-to-clean-up-entities-in-savechanges"></a>Použití místní k vyčištění entit v SaveChanges  

Ve většině případů se entity odebrané z navigační vlastnosti nebudou automaticky označovat jako odstraněné v kontextu. Například odeberete-li objekt post z blogu. po volání metody SaveChanges nebude tento příspěvek automaticky odstraněn. Pokud ho potřebujete odstranit, možná budete muset najít tyto entity dangling a označit je jako odstraněné před voláním metody SaveChanges nebo jako součást přepsaného metody SaveChanges. Příklad:  

``` csharp
public override int SaveChanges()
{
    foreach (var post in this.Posts.Local.ToList())
    {
        if (post.Blog == null)
        {
            this.Posts.Remove(post);
        }
    }

    return base.SaveChanges();
}
```  

Výše uvedený kód používá místní kolekci k vyhledání všech příspěvků a označení, které nemají odkaz na blog jako odstraněný. Volání ToList – je požadováno, protože v opačném případě bude kolekce upravena voláním Remove během vytváření výčtu. Ve většině případů se můžete dotazovat přímo na místní vlastnost bez použití ToList – jako prvního.  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a>Použití místních a ToBindingList pro datovou vazbu model Windows Forms  

Model Windows Forms nepodporuje přímou datovou vazbu pomocí kolekci ObservableCollection. K získání všech výhod popsaných v předchozích částech však stále můžete použít místní vlastnost Negenerickými pro datovou vazbu. Toho dosáhnete pomocí metody rozšíření ToBindingList, která vytvoří implementaci [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) , kterou zajišťuje místní kolekci ObservableCollection.  

Nejedná se o vhodné místo pro úplnou model Windows Forms ukázku datových vazeb, ale klíčové prvky jsou:  

- Nastavení zdroje vazby objektu  
- Vytvořte jeho svázání s místní vlastností vaší sady pomocí Local. ToBindingList ().  
- Naplnit místní pomocí dotazu do databáze  

## <a name="getting-detailed-information-about-tracked-entities"></a>Získání podrobných informací o sledovaných entitách  

Mnohé z příkladů v této sérii používají metodu entry k vrácení instance DbEntityEntry pro entitu. Tento objekt vstupu pak slouží jako výchozí bod pro shromažďování informací o entitě, jako je její aktuální stav, a také pro provádění operací na entitě, jako je například explicitní načtení související entity.  

Metody záznamů vrací objekty DbEntityEntry pro mnoho nebo všechny entity, které jsou sledovány kontextem. Díky tomu můžete shromažďovat informace nebo provádět operace mnoha entit, a ne jenom jednu položku. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    // Load some entities into the context
    context.Blogs.Load();
    context.Authors.Load();
    context.Readers.Load();

    // Make some changes
    context.Blogs.Find(1).Title = "The New ADO.NET Blog";
    context.Blogs.Remove(context.Blogs.Find(2));
    context.Authors.Add(new Author { Name = "Jane Doe" });
    context.Readers.Find(1).Username = "johndoe1987";

    // Look at the state of all entities in the context
    Console.WriteLine("All tracked entities: ");
    foreach (var entry in context.ChangeTracker.Entries())
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Find modified entities of any type
    Console.WriteLine("\nAll modified entities: ");
    foreach (var entry in context.ChangeTracker.Entries()
                              .Where(e => e.State == EntityState.Modified))
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Get some information about just the tracked blogs
    Console.WriteLine("\nTracked blogs: ");
    foreach (var entry in context.ChangeTracker.Entries<Blog>())
    {
        Console.WriteLine(
            "Found Blog {0}: {1} with original Name {2}",
            entry.Entity.BlogId,  
            entry.Entity.Name,
            entry.Property(p => p.Name).OriginalValue);
    }

    // Find all people (author or reader)
    Console.WriteLine("\nPeople: ");
    foreach (var entry in context.ChangeTracker.Entries<IPerson>())
    {
        Console.WriteLine("Found Person {0}", entry.Entity.Name);
    }
}
```  

Všimnete si, že zavádíme třídu autora a čtenáře do příkladu – obě tyto třídy implementují rozhraní IPerson.  

``` csharp
public class Author : IPerson
{
    public int AuthorId { get; set; }
    public string Name { get; set; }
    public string Biography { get; set; }
}

public class Reader : IPerson
{
    public int ReaderId { get; set; }
    public string Name { get; set; }
    public string Username { get; set; }
}

public interface IPerson
{
    string Name { get; }
}
```  

Řekněme, že v databázi máme následující data:

Blog s BlogId = 1 a name = ' ADO.NET blog '  
Blog s BlogId = 2 a name = ' blog sady Visual Studio '  
Blog s BlogId = 3 a názvem = ' .NET Framework blog '  
Autor s AuthorId = 1 a name = ' Jan Bloggs '  
Čtecí modul s ReaderId = 1 a name = Jan Chvojková  

Výstup z běhu kódu by byl:  

```console
All tracked entities:
Found entity of type Blog with state Modified
Found entity of type Blog with state Deleted
Found entity of type Blog with state Unchanged
Found entity of type Author with state Unchanged
Found entity of type Author with state Added
Found entity of type Reader with state Modified

All modified entities:
Found entity of type Blog with state Modified
Found entity of type Reader with state Modified

Tracked blogs:
Found Blog 1: The New ADO.NET Blog with original Name ADO.NET Blog
Found Blog 2: The Visual Studio Blog with original Name The Visual Studio Blog
Found Blog 3: .NET Framework Blog with original Name .NET Framework Blog

People:
Found Person John Doe
Found Person Joe Bloggs
Found Person Jane Doe
```  

Tyto příklady ilustrují několik bodů:  

- Metody záznamů vrací položky pro entity ve všech stavech, včetně odstranění. Porovnat tuto místní a vyloučí odstraněné entity.  
- Položky pro všechny typy entit jsou vráceny při použití metody neobecného typu. Když je metoda obecných záznamů použita, vrátí se pouze pro entity, které jsou instancemi obecného typu. Výše se použila k získání položek pro všechny blogy. Byl také použit k získání položek pro všechny entity, které implementují IPerson. To ukazuje, že obecný typ nemusí být skutečný typ entity.  
- LINQ to Objects lze použít k filtrování vrácených výsledků. Toto bylo použito výše pro vyhledání entit libovolného typu, pokud jsou upraveny.  

Všimněte si, že instance DbEntityEntry vždy obsahují entitu, která není null. Položky vztahů a zástupné procedury nejsou reprezentovány jako instance DbEntityEntry, takže je není nutné vyfiltrovat.
