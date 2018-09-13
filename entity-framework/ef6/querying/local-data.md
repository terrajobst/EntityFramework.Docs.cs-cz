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
# <a name="local-data"></a><span data-ttu-id="92005-102">Místní Data</span><span class="sxs-lookup"><span data-stu-id="92005-102">Local Data</span></span>
<span data-ttu-id="92005-103">Spuštění dotazu LINQ přímo proti DbSet, bude vždy odesílat dotaz do databáze, ale přistupujete k datům, která je aktuálně v paměti pomocí vlastnosti DbSet.Local.</span><span class="sxs-lookup"><span data-stu-id="92005-103">Running a LINQ query directly against a DbSet will always send a query to the database, but you can access the data that is currently in-memory using the DbSet.Local property.</span></span> <span data-ttu-id="92005-104">Další informace, které sleduje EF dostanete také o entit pomocí metody DbContext.Entry a DbContext.ChangeTracker.Entries.</span><span class="sxs-lookup"><span data-stu-id="92005-104">You can also access the extra information EF is tracking about your entities using the DbContext.Entry and DbContext.ChangeTracker.Entries methods.</span></span> <span data-ttu-id="92005-105">Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.</span><span class="sxs-lookup"><span data-stu-id="92005-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="using-local-to-look-at-local-data"></a><span data-ttu-id="92005-106">Podívejte se na místní data pomocí místní</span><span class="sxs-lookup"><span data-stu-id="92005-106">Using Local to look at local data</span></span>  

<span data-ttu-id="92005-107">Místní vlastnost DbSet poskytuje snadný přístup k entitám sady, které jsou právě sledován správou kontextu a ještě nebyly označeny jako odstraněný.</span><span class="sxs-lookup"><span data-stu-id="92005-107">The Local property of DbSet provides simple access to the entities of the set that are currently being tracked by the context and have not been marked as Deleted.</span></span> <span data-ttu-id="92005-108">Přístup k místní vlastnost nikdy nezpůsobí dotaz k odeslání do databáze.</span><span class="sxs-lookup"><span data-stu-id="92005-108">Accessing the Local property never causes a query to be sent to the database.</span></span> <span data-ttu-id="92005-109">To znamená, že se obvykle používá po dotaz už jsou hotové.</span><span class="sxs-lookup"><span data-stu-id="92005-109">This means that it is usually used after a query has already been performed.</span></span> <span data-ttu-id="92005-110">Rozšiřující metoda zatížení lze použít k provedení dotazu tak, aby kontext sleduje výsledky.</span><span class="sxs-lookup"><span data-stu-id="92005-110">The Load extension method can be used to execute a query so that the context tracks the results.</span></span> <span data-ttu-id="92005-111">Příklad:</span><span class="sxs-lookup"><span data-stu-id="92005-111">For example:</span></span>  

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

<span data-ttu-id="92005-112">Kdybychom měli dva blogy v databázi – "ADO.NET Blog" BlogId 1 - a "The Visual Studio Blog" BlogId 2 očekávat následující výstup:</span><span class="sxs-lookup"><span data-stu-id="92005-112">If we had two blogs in the database - 'ADO.NET Blog' with a BlogId of 1 and 'The Visual Studio Blog' with a BlogId of 2 - we could expect the following output:</span></span>  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

<span data-ttu-id="92005-113">To ukazuje tři body:</span><span class="sxs-lookup"><span data-stu-id="92005-113">This illustrates three points:</span></span>  

- <span data-ttu-id="92005-114">Nového blogu "My nového blogu" je součástí místního shromažďování dat, i když ještě nebyly uloženy do databáze.</span><span class="sxs-lookup"><span data-stu-id="92005-114">The new blog 'My New Blog' is included in the Local collection even though it has not yet been saved to the database.</span></span> <span data-ttu-id="92005-115">Tento blog nemá primární klíč nula, protože databáze nebyla zatím nevygenerovaly skutečné klíč entity.</span><span class="sxs-lookup"><span data-stu-id="92005-115">This blog has a primary key of zero because the database has not yet generated a real key for the entity.</span></span>  
- <span data-ttu-id="92005-116">V blogu ADO.NET není součástí místního shromažďování dat i v případě, že je stále sledována kontextu.</span><span class="sxs-lookup"><span data-stu-id="92005-116">The 'ADO.NET Blog' is not included in the local collection even though it is still being tracked by the context.</span></span> <span data-ttu-id="92005-117">Je to proto, že jsme odebrali z DbSet, a tím označit ji jako odstraněný.</span><span class="sxs-lookup"><span data-stu-id="92005-117">This is because we removed it from the DbSet thereby marking it as deleted.</span></span>  
- <span data-ttu-id="92005-118">Při použití DbSet provést dotaz na blogu označená k odstranění (ADO.NET Blog) je zahrnut ve výsledcích a nového blogu (Moje nového blogu), které ještě nebyly uloženy do databáze není zahrnut ve výsledcích.</span><span class="sxs-lookup"><span data-stu-id="92005-118">When DbSet is used to perform a query the blog marked for deletion (ADO.NET Blog) is included in the results and the new blog (My New Blog) that has not yet been saved to the database is not included in the results.</span></span> <span data-ttu-id="92005-119">Je to proto DbSet provádí dotaz na databázi a výsledky vrácené vždy odráží, co je v databázi.</span><span class="sxs-lookup"><span data-stu-id="92005-119">This is because DbSet is performing a query against the database and the results returned always reflect what is in the database.</span></span>  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a><span data-ttu-id="92005-120">Pomocí místní přidávat a odebírat entity z kontextu</span><span class="sxs-lookup"><span data-stu-id="92005-120">Using Local to add and remove entities from the context</span></span>  

<span data-ttu-id="92005-121">Vrátí místní vlastnosti na DbSet [kolekci ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) s událostmi připojili tak, že stále synchronizovaná s obsahem kontextu.</span><span class="sxs-lookup"><span data-stu-id="92005-121">The Local property on DbSet returns an [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) with events hooked up such that it stays in sync with the contents of the context.</span></span> <span data-ttu-id="92005-122">To znamená, že můžete přidat nebo odebrat z místního shromažďování dat nebo DbSet entity.</span><span class="sxs-lookup"><span data-stu-id="92005-122">This means that entities can be added or removed from either the Local collection or the DbSet.</span></span> <span data-ttu-id="92005-123">Také znamená, že výsledkem dotazů, které přinášejí nové entity do kontextu bude místního shromažďování dat aktualizovány s těmito entitami.</span><span class="sxs-lookup"><span data-stu-id="92005-123">It also means that queries that bring new entities into the context will result in the Local collection being updated with those entities.</span></span> <span data-ttu-id="92005-124">Příklad:</span><span class="sxs-lookup"><span data-stu-id="92005-124">For example:</span></span>  

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

<span data-ttu-id="92005-125">Za předpokladu, že jsme měli několik příspěvky označené 'entity framework' a 'asp.net"výstup může vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="92005-125">Assuming we had a few posts tagged with 'entity-framework' and 'asp.net' the output may look something like this:</span></span>  

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

<span data-ttu-id="92005-126">To ukazuje tři body:</span><span class="sxs-lookup"><span data-stu-id="92005-126">This illustrates three points:</span></span>  

- <span data-ttu-id="92005-127">Nový příspěvek "Co je nového v EF", který byl přidán do místní kolekce bude sledován pomocí funkce kontextu, ve stavu Added.</span><span class="sxs-lookup"><span data-stu-id="92005-127">The new post 'What's New in EF' that was added to the Local collection becomes tracked by the context in the Added state.</span></span> <span data-ttu-id="92005-128">Je proto vloží do databáze když je volána metoda SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="92005-128">It will therefore be inserted into the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="92005-129">Příspěvku, který byl odebrán z místní kolekce (Průvodce začátečníka EF) je nyní označen jako odstraněný v kontextu.</span><span class="sxs-lookup"><span data-stu-id="92005-129">The post that was removed from the Local collection (EF Beginners Guide) is now marked as deleted in the context.</span></span> <span data-ttu-id="92005-130">Proto se odstraní z databáze když je volána metoda SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="92005-130">It will therefore be deleted from the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="92005-131">Další příspěvek (Průvodce začátečníka technologie ASP.NET) načítají do kontextu s druhého dotazu se automaticky přidá do místní kolekce.</span><span class="sxs-lookup"><span data-stu-id="92005-131">The additional post (ASP.NET Beginners Guide) loaded into the context with the second query is automatically added to the Local collection.</span></span>  

<span data-ttu-id="92005-132">Jeden poslední věc, kterou si uvědomit o místní je, že, protože se jedná výkonu kolekci ObservableCollection není velmi vhodné pro velký počet entit.</span><span class="sxs-lookup"><span data-stu-id="92005-132">One final thing to note about Local is that because it is an ObservableCollection performance is not great for large numbers of entities.</span></span> <span data-ttu-id="92005-133">Proto pokud pracujete s tisíci entitami ve vaší místní nemusí být vhodné použít místní.</span><span class="sxs-lookup"><span data-stu-id="92005-133">Therefore if you are dealing with thousands of entities in your context it may not be advisable to use Local.</span></span>  

## <a name="using-local-for-wpf-data-binding"></a><span data-ttu-id="92005-134">Pomocí místní datové vazby WPF</span><span class="sxs-lookup"><span data-stu-id="92005-134">Using Local for WPF data binding</span></span>  

<span data-ttu-id="92005-135">Místní vlastnosti na DbSet můžete použít přímo pro datové vazby v aplikaci WPF, protože je instance kolekci ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="92005-135">The Local property on DbSet can be used directly for data binding in a WPF application because it is an instance of ObservableCollection.</span></span> <span data-ttu-id="92005-136">Jak je popsáno v předchozí části, to znamená, že bude automaticky zůstávat synchronizovaný s obsahem kontextu a obsahu kontextu bude automaticky Zůstaňte synchronizovaný s ním.</span><span class="sxs-lookup"><span data-stu-id="92005-136">As described in the previous sections this means that it will automatically stay in sync with the contents of the context and the contents of the context will automatically stay in sync with it.</span></span> <span data-ttu-id="92005-137">Všimněte si, že potřebujete k předběžnému naplnění místní shromažďování dat pro něj být cokoliv, k vytvoření vazby, protože místní nikdy nezpůsobí databázového dotazu.</span><span class="sxs-lookup"><span data-stu-id="92005-137">Note that you do need to pre-populate the Local collection with data for there to be anything to bind to since Local never causes a database query.</span></span>  

<span data-ttu-id="92005-138">Toto není vhodné místo jako úplnou ukázku datové vazby WPF, ale jsou klíčové prvky:</span><span class="sxs-lookup"><span data-stu-id="92005-138">This is not an appropriate place for a full WPF data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="92005-139">Nastavení zdroje vazby</span><span class="sxs-lookup"><span data-stu-id="92005-139">Setup a binding source</span></span>  
- <span data-ttu-id="92005-140">Vázat na místní vlastnost sady</span><span class="sxs-lookup"><span data-stu-id="92005-140">Bind it to the Local property of your set</span></span>  
- <span data-ttu-id="92005-141">Naplnění místní pomocí dotazu do databáze.</span><span class="sxs-lookup"><span data-stu-id="92005-141">Populate Local using a query to the database.</span></span>  

## <a name="wpf-binding-to-navigation-properties"></a><span data-ttu-id="92005-142">WPF vazby pro navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="92005-142">WPF binding to navigation properties</span></span>  

<span data-ttu-id="92005-143">Pokud provádíte záznamů master/detail datové vazby, které může chcete svázat zobrazení podrobností pro navigační vlastnost jednoho z entit.</span><span class="sxs-lookup"><span data-stu-id="92005-143">If you are doing master/detail data binding you may want to bind the detail view to a navigation property of one of your entities.</span></span> <span data-ttu-id="92005-144">Snadný způsob, jak provést tuto práci je použití ObservableCollection pro navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="92005-144">An easy way to make this work is to use an ObservableCollection for the navigation property.</span></span> <span data-ttu-id="92005-145">Příklad:</span><span class="sxs-lookup"><span data-stu-id="92005-145">For example:</span></span>  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a><span data-ttu-id="92005-146">Pomocí místní vyčistit entity v SaveChanges</span><span class="sxs-lookup"><span data-stu-id="92005-146">Using Local to clean up entities in SaveChanges</span></span>  

<span data-ttu-id="92005-147">Ve většině případů entity odebrána z navigační vlastnost nebude označeno automaticky odstraněn v kontextu.</span><span class="sxs-lookup"><span data-stu-id="92005-147">In most cases entities removed from a navigation property will not be automatically marked as deleted in the context.</span></span> <span data-ttu-id="92005-148">Například pokud odeberete příspěvek objekt z kolekce Blog.Posts pak příspěvku, který nebude automaticky odstraněno při je volána metoda SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="92005-148">For example, if you remove a Post object from the Blog.Posts collection then that post will not be automatically deleted when SaveChanges is called.</span></span> <span data-ttu-id="92005-149">Pokud je třeba ho odstraníme může musíte najít tyto nepropojená entity a označit je jako odstraněný před voláním SaveChanges nebo jako součást přepsané SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="92005-149">If you need it to be deleted then you may need to find these dangling entities and mark them as deleted before calling SaveChanges or as part of an overridden SaveChanges.</span></span> <span data-ttu-id="92005-150">Příklad:</span><span class="sxs-lookup"><span data-stu-id="92005-150">For example:</span></span>  

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

<span data-ttu-id="92005-151">Výše uvedený kód místního shromažďování dat používá k nalezení všech příspěvků a značky, jež nemají odkaz blogu jako odstraněný.</span><span class="sxs-lookup"><span data-stu-id="92005-151">The code above uses the Local collection to find all posts and marks any that do not have a blog reference as deleted.</span></span> <span data-ttu-id="92005-152">ToList – volání je nutné, protože jinak kolekce se upravil odebrat volání, když je zpracován.</span><span class="sxs-lookup"><span data-stu-id="92005-152">The ToList call is required because otherwise the collection will be modified by the Remove call while it is being enumerated.</span></span> <span data-ttu-id="92005-153">Ve většině případů další se můžete dotazovat přímo proti místní vlastnost bez použití ToList – první.</span><span class="sxs-lookup"><span data-stu-id="92005-153">In most other situations you can query directly against the Local property without using ToList first.</span></span>  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a><span data-ttu-id="92005-154">Použití datových vazeb místní verzi a ToBindingList pro Windows Forms</span><span class="sxs-lookup"><span data-stu-id="92005-154">Using Local and ToBindingList for Windows Forms data binding</span></span>  

<span data-ttu-id="92005-155">Windows Forms nepodporuje plnou věrností datové vazby pomocí kolekci ObservableCollection přímo.</span><span class="sxs-lookup"><span data-stu-id="92005-155">Windows Forms does not support full fidelity data binding using ObservableCollection directly.</span></span> <span data-ttu-id="92005-156">Nicméně stále můžete DbSet místní vlastnost pro vytváření datových vazeb získáte všechny výhody, které jsou popsané v předchozích částech.</span><span class="sxs-lookup"><span data-stu-id="92005-156">However, you can still use the DbSet Local property for data binding to get all the benefits described in the previous sections.</span></span> <span data-ttu-id="92005-157">Toho můžete dosáhnout ToBindingList metodu rozšíření, která vytvoří [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementace se opírá o místní kolekci ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="92005-157">This is achieved through the ToBindingList extension method which creates an [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementation backed by the Local ObservableCollection.</span></span>  

<span data-ttu-id="92005-158">Toto není vhodné místo pro úplnou ukázku vazby dat formuláře Windows, ale jsou klíčové prvky:</span><span class="sxs-lookup"><span data-stu-id="92005-158">This is not an appropriate place for a full Windows Forms data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="92005-159">Nastavit zdroj vazby objektu</span><span class="sxs-lookup"><span data-stu-id="92005-159">Setup an object binding source</span></span>  
- <span data-ttu-id="92005-160">Vázat na místní vlastnost sady pomocí Local.ToBindingList()</span><span class="sxs-lookup"><span data-stu-id="92005-160">Bind it to the Local property of your set using Local.ToBindingList()</span></span>  
- <span data-ttu-id="92005-161">Naplnění místní pomocí dotazu do databáze</span><span class="sxs-lookup"><span data-stu-id="92005-161">Populate Local using a query to the database</span></span>  

## <a name="getting-detailed-information-about-tracked-entities"></a><span data-ttu-id="92005-162">Získání podrobných informací o sledované entity</span><span class="sxs-lookup"><span data-stu-id="92005-162">Getting detailed information about tracked entities</span></span>  

<span data-ttu-id="92005-163">Mnoho příkladů v této sérii použít metodu vstupního vrácení DbEntityEntry instance entity.</span><span class="sxs-lookup"><span data-stu-id="92005-163">Many of the examples in this series use the Entry method to return a DbEntityEntry instance for an entity.</span></span> <span data-ttu-id="92005-164">Tento objekt záznam pak funguje jako výchozí bod pro shromažďování informací o entitě, jako je například jejím aktuálním stavu, stejně jako pro provádění operací na entitu, jako je například explicitně načtení souvisejících entit.</span><span class="sxs-lookup"><span data-stu-id="92005-164">This entry object then acts as the starting point for gathering information about the entity such as its current state, as well as for performing operations on the entity such as explicitly loading a related entity.</span></span>  

<span data-ttu-id="92005-165">Položky metody vrací objekty DbEntityEntry pro mnoho nebo všechna entity, sledován správou kontextu.</span><span class="sxs-lookup"><span data-stu-id="92005-165">The Entries methods return DbEntityEntry objects for many or all entities being tracked by the context.</span></span> <span data-ttu-id="92005-166">To umožňuje shromažďování informací nebo provádění operací na mnoho entit a nikoli pouze jedna položka.</span><span class="sxs-lookup"><span data-stu-id="92005-166">This allows you to gather information or perform operations on many entities rather than just a single entry.</span></span> <span data-ttu-id="92005-167">Příklad:</span><span class="sxs-lookup"><span data-stu-id="92005-167">For example:</span></span>  

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

<span data-ttu-id="92005-168">Můžete si všimnout Představujeme Autor a čtečky třídy do příkladu – obě tyto třídy implementace rozhraní IPerson.</span><span class="sxs-lookup"><span data-stu-id="92005-168">You'll notice we are introducing a Author and Reader class into the example - both of these classes implement the IPerson interface.</span></span>  

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

<span data-ttu-id="92005-169">Předpokládejme, že máme následující data v databázi:</span><span class="sxs-lookup"><span data-stu-id="92005-169">Let's assume we have the following data in the database:</span></span>

<span data-ttu-id="92005-170">Blog s BlogId = 1 a Name = "ADO.NET blogu.</span><span class="sxs-lookup"><span data-stu-id="92005-170">Blog with BlogId = 1 and Name = 'ADO.NET Blog'</span></span>  
<span data-ttu-id="92005-171">Blog s BlogId = 2 a Name = "v blogu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="92005-171">Blog with BlogId = 2 and Name = 'The Visual Studio Blog'</span></span>  
<span data-ttu-id="92005-172">Blog s BlogId = 3 a Name = "Blog k .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="92005-172">Blog with BlogId = 3 and Name = '.NET Framework Blog'</span></span>  
<span data-ttu-id="92005-173">Autor s AuthorId = 1 a Name = "Joe Bloggs.</span><span class="sxs-lookup"><span data-stu-id="92005-173">Author with AuthorId = 1 and Name = 'Joe Bloggs'</span></span>  
<span data-ttu-id="92005-174">Čtečka s ReaderId = 1 a Name = "John Doe"</span><span class="sxs-lookup"><span data-stu-id="92005-174">Reader with ReaderId = 1 and Name = 'John Doe'</span></span>  

<span data-ttu-id="92005-175">Výstup spuštění kódu by byl:</span><span class="sxs-lookup"><span data-stu-id="92005-175">The output from running the code would be:</span></span>  

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

<span data-ttu-id="92005-176">Tyto příklady ilustrují několik bodů:</span><span class="sxs-lookup"><span data-stu-id="92005-176">These examples illustrate several points:</span></span>  

- <span data-ttu-id="92005-177">Položky metody vrací záznamy pro entity v všechny stavy, včetně odstraněno.</span><span class="sxs-lookup"><span data-stu-id="92005-177">The Entries methods return entries for entities in all states, including Deleted.</span></span> <span data-ttu-id="92005-178">Porovnejte tuto hodnotu na místní, která nezahrnuje odstranit entity.</span><span class="sxs-lookup"><span data-stu-id="92005-178">Compare this to Local which excludes Deleted entities.</span></span>  
- <span data-ttu-id="92005-179">Záznamy pro všechny typy entit se vrátí, když se používá neobecnou metodu položky.</span><span class="sxs-lookup"><span data-stu-id="92005-179">Entries for all entity types are returned when the non-generic Entries method is used.</span></span> <span data-ttu-id="92005-180">Při použití metody obecných položek položky jsou vráceny pouze pro entity, které jsou instancemi obecného typu.</span><span class="sxs-lookup"><span data-stu-id="92005-180">When the generic entries method is used entries are only returned for entities that are instances of the generic type.</span></span> <span data-ttu-id="92005-181">To byl použit k získání položek pro všechny blogy výše.</span><span class="sxs-lookup"><span data-stu-id="92005-181">This was used above to get entries for all blogs.</span></span> <span data-ttu-id="92005-182">Používá se také k získání položek pro všechny entity, které implementují IPerson.</span><span class="sxs-lookup"><span data-stu-id="92005-182">It was also used to get entries for all entities that implement IPerson.</span></span> <span data-ttu-id="92005-183">Tento příklad ukazuje, že obecný typ nemusí být skutečný entity typu.</span><span class="sxs-lookup"><span data-stu-id="92005-183">This demonstrates that the generic type does not have to be an actual entity type.</span></span>  
- <span data-ttu-id="92005-184">LINQ na objekty je možné filtrovat vrácené výsledky.</span><span class="sxs-lookup"><span data-stu-id="92005-184">LINQ to Objects can be used to filter the results returned.</span></span> <span data-ttu-id="92005-185">Byl použit nad najít entity libovolného typu, tak dlouho, dokud jejich úpravou.</span><span class="sxs-lookup"><span data-stu-id="92005-185">This was used above to find entities of any type as long as they are modified.</span></span>  

<span data-ttu-id="92005-186">Všimněte si, že instance DbEntityEntry vždy obsahují Entity jinou hodnotu než null.</span><span class="sxs-lookup"><span data-stu-id="92005-186">Note that DbEntityEntry instances always contain a non-null Entity.</span></span> <span data-ttu-id="92005-187">Vztah položek a zástupných procedur nejsou reprezentovány jako DbEntityEntry instance, není nutné k filtrování pro tyto.</span><span class="sxs-lookup"><span data-stu-id="92005-187">Relationship entries and stub entries are not represented as DbEntityEntry instances so there is no need to filter for these.</span></span>
