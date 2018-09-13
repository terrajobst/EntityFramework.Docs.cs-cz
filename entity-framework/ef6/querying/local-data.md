---
title: Místní Data - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: 400b9e1337edac1b9fa4f0ec9e1384ca58aa2fbc
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490451"
---
# <a name="local-data"></a>Místní Data
Spuštění dotazu LINQ přímo proti DbSet, bude vždy odesílat dotaz do databáze, ale přistupujete k datům, která je aktuálně v paměti pomocí vlastnosti DbSet.Local. Další informace, které sleduje EF dostanete také o entit pomocí metody DbContext.Entry a DbContext.ChangeTracker.Entries. Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.  

## <a name="using-local-to-look-at-local-data"></a>Podívejte se na místní data pomocí místní  

Místní vlastnost DbSet poskytuje snadný přístup k entitám sady, které jsou právě sledován správou kontextu a ještě nebyly označeny jako odstraněný. Přístup k místní vlastnost nikdy nezpůsobí dotaz k odeslání do databáze. To znamená, že se obvykle používá po dotaz už jsou hotové. Rozšiřující metoda zatížení lze použít k provedení dotazu tak, aby kontext sleduje výsledky. Příklad:  

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

Kdybychom měli dva blogy v databázi – "ADO.NET Blog" BlogId 1 - a "The Visual Studio Blog" BlogId 2 očekávat následující výstup:  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

To ukazuje tři body:  

- Nového blogu "My nového blogu" je součástí místního shromažďování dat, i když ještě nebyly uloženy do databáze. Tento blog nemá primární klíč nula, protože databáze nebyla zatím nevygenerovaly skutečné klíč entity.  
- V blogu ADO.NET není součástí místního shromažďování dat i v případě, že je stále sledována kontextu. Je to proto, že jsme odebrali z DbSet, a tím označit ji jako odstraněný.  
- Při použití DbSet provést dotaz na blogu označená k odstranění (ADO.NET Blog) je zahrnut ve výsledcích a nového blogu (Moje nového blogu), které ještě nebyly uloženy do databáze není zahrnut ve výsledcích. Je to proto DbSet provádí dotaz na databázi a výsledky vrácené vždy odráží, co je v databázi.  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a>Pomocí místní přidávat a odebírat entity z kontextu  

Vrátí místní vlastnosti na DbSet [kolekci ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) s událostmi připojili tak, že stále synchronizovaná s obsahem kontextu. To znamená, že můžete přidat nebo odebrat z místního shromažďování dat nebo DbSet entity. Také znamená, že výsledkem dotazů, které přinášejí nové entity do kontextu bude místního shromažďování dat aktualizovány s těmito entitami. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework").Load();  

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

Za předpokladu, že jsme měli několik příspěvky označené 'entity framework' a 'asp.net"výstup může vypadat přibližně takto:  

```  
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

- Nový příspěvek "Co je nového v EF", který byl přidán do místní kolekce bude sledován pomocí funkce kontextu, ve stavu Added. Je proto vloží do databáze když je volána metoda SaveChanges.  
- Příspěvku, který byl odebrán z místní kolekce (Průvodce začátečníka EF) je nyní označen jako odstraněný v kontextu. Proto se odstraní z databáze když je volána metoda SaveChanges.  
- Další příspěvek (Průvodce začátečníka technologie ASP.NET) načítají do kontextu s druhého dotazu se automaticky přidá do místní kolekce.  

Jeden poslední věc, kterou si uvědomit o místní je, že, protože se jedná výkonu kolekci ObservableCollection není velmi vhodné pro velký počet entit. Proto pokud pracujete s tisíci entitami ve vaší místní nemusí být vhodné použít místní.  

## <a name="using-local-for-wpf-data-binding"></a>Pomocí místní datové vazby WPF  

Místní vlastnosti na DbSet můžete použít přímo pro datové vazby v aplikaci WPF, protože je instance kolekci ObservableCollection. Jak je popsáno v předchozí části, to znamená, že bude automaticky zůstávat synchronizovaný s obsahem kontextu a obsahu kontextu bude automaticky Zůstaňte synchronizovaný s ním. Všimněte si, že potřebujete k předběžnému naplnění místní shromažďování dat pro něj být cokoliv, k vytvoření vazby, protože místní nikdy nezpůsobí databázového dotazu.  

Toto není vhodné místo jako úplnou ukázku datové vazby WPF, ale jsou klíčové prvky:  

- Nastavení zdroje vazby  
- Vázat na místní vlastnost sady  
- Naplnění místní pomocí dotazu do databáze.  

## <a name="wpf-binding-to-navigation-properties"></a>WPF vazby pro navigační vlastnosti  

Pokud provádíte záznamů master/detail datové vazby, které může chcete svázat zobrazení podrobností pro navigační vlastnost jednoho z entit. Snadný způsob, jak provést tuto práci je použití ObservableCollection pro navigační vlastnost. Příklad:  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a>Pomocí místní vyčistit entity v SaveChanges  

Ve většině případů entity odebrána z navigační vlastnost nebude označeno automaticky odstraněn v kontextu. Například pokud odeberete příspěvek objekt z kolekce Blog.Posts pak příspěvku, který nebude automaticky odstraněno při je volána metoda SaveChanges. Pokud je třeba ho odstraníme může musíte najít tyto nepropojená entity a označit je jako odstraněný před voláním SaveChanges nebo jako součást přepsané SaveChanges. Příklad:  

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

Výše uvedený kód místního shromažďování dat používá k nalezení všech příspěvků a značky, jež nemají odkaz blogu jako odstraněný. ToList – volání je nutné, protože jinak kolekce se upravil odebrat volání, když je zpracován. Ve většině případů další se můžete dotazovat přímo proti místní vlastnost bez použití ToList – první.  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a>Použití datových vazeb místní verzi a ToBindingList pro Windows Forms  

Windows Forms nepodporuje plnou věrností datové vazby pomocí kolekci ObservableCollection přímo. Nicméně stále můžete DbSet místní vlastnost pro vytváření datových vazeb získáte všechny výhody, které jsou popsané v předchozích částech. Toho můžete dosáhnout ToBindingList metodu rozšíření, která vytvoří [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementace se opírá o místní kolekci ObservableCollection.  

Toto není vhodné místo pro úplnou ukázku vazby dat formuláře Windows, ale jsou klíčové prvky:  

- Nastavit zdroj vazby objektu  
- Vázat na místní vlastnost sady pomocí Local.ToBindingList()  
- Naplnění místní pomocí dotazu do databáze  

## <a name="getting-detailed-information-about-tracked-entities"></a>Získání podrobných informací o sledované entity  

Mnoho příkladů v této sérii použít metodu vstupního vrácení DbEntityEntry instance entity. Tento objekt záznam pak funguje jako výchozí bod pro shromažďování informací o entitě, jako je například jejím aktuálním stavu, stejně jako pro provádění operací na entitu, jako je například explicitně načtení souvisejících entit.  

Položky metody vrací objekty DbEntityEntry pro mnoho nebo všechna entity, sledován správou kontextu. To umožňuje shromažďování informací nebo provádění operací na mnoho entit a nikoli pouze jedna položka. Příklad:  

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

Můžete si všimnout Představujeme Autor a čtečky třídy do příkladu – obě tyto třídy implementace rozhraní IPerson.  

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

Předpokládejme, že máme následující data v databázi:

Blog s BlogId = 1 a Name = "ADO.NET blogu.  
Blog s BlogId = 2 a Name = "v blogu Visual Studio.  
Blog s BlogId = 3 a Name = "Blog k .NET Framework.  
Autor s AuthorId = 1 a Name = "Joe Bloggs.  
Čtečka s ReaderId = 1 a Name = "John Doe"  

Výstup spuštění kódu by byl:  

```  
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

- Položky metody vrací záznamy pro entity v všechny stavy, včetně odstraněno. Porovnejte tuto hodnotu na místní, která nezahrnuje odstranit entity.  
- Záznamy pro všechny typy entit se vrátí, když se používá neobecnou metodu položky. Při použití metody obecných položek položky jsou vráceny pouze pro entity, které jsou instancemi obecného typu. To byl použit k získání položek pro všechny blogy výše. Používá se také k získání položek pro všechny entity, které implementují IPerson. Tento příklad ukazuje, že obecný typ nemusí být skutečný entity typu.  
- LINQ na objekty je možné filtrovat vrácené výsledky. Byl použit nad najít entity libovolného typu, tak dlouho, dokud jejich úpravou.  

Všimněte si, že instance DbEntityEntry vždy obsahují Entity jinou hodnotu než null. Vztah položek a zástupných procedur nejsou reprezentovány jako DbEntityEntry instance, není nutné k filtrování pro tyto.
