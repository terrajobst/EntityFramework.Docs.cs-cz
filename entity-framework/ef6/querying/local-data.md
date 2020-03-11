---
title: Místní data – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: efd646348d8a18bbeed2d0a0e708d4d36eb26eac
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417107"
---
# <a name="local-data"></a><span data-ttu-id="6330b-102">Místní data</span><span class="sxs-lookup"><span data-stu-id="6330b-102">Local Data</span></span>
<span data-ttu-id="6330b-103">Spuštění dotazu LINQ přímo proti Negenerickými vždy pošle dotaz do databáze, ale přístup k datům, která jsou aktuálně v paměti, můžete získat pomocí vlastnosti Negenerickými. Local.</span><span class="sxs-lookup"><span data-stu-id="6330b-103">Running a LINQ query directly against a DbSet will always send a query to the database, but you can access the data that is currently in-memory using the DbSet.Local property.</span></span> <span data-ttu-id="6330b-104">Přístup k základnímu EF informací získáte také tak, že sledujete své entity pomocí metod DbContext. entry a DbContext. ChangeTracker. Entries.</span><span class="sxs-lookup"><span data-stu-id="6330b-104">You can also access the extra information EF is tracking about your entities using the DbContext.Entry and DbContext.ChangeTracker.Entries methods.</span></span> <span data-ttu-id="6330b-105">Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="6330b-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="using-local-to-look-at-local-data"></a><span data-ttu-id="6330b-106">Použití místní k zobrazení místních dat</span><span class="sxs-lookup"><span data-stu-id="6330b-106">Using Local to look at local data</span></span>  

<span data-ttu-id="6330b-107">Místní vlastnost Negenerickými poskytuje jednoduchý přístup k entitám sady, které jsou aktuálně sledovány pomocí kontextu a nebyly označeny jako odstraněné.</span><span class="sxs-lookup"><span data-stu-id="6330b-107">The Local property of DbSet provides simple access to the entities of the set that are currently being tracked by the context and have not been marked as Deleted.</span></span> <span data-ttu-id="6330b-108">Přístup k místní vlastnosti nikdy nevede k odeslání dotazu do databáze.</span><span class="sxs-lookup"><span data-stu-id="6330b-108">Accessing the Local property never causes a query to be sent to the database.</span></span> <span data-ttu-id="6330b-109">To znamená, že se obvykle používá poté, co byl dotaz již proveden.</span><span class="sxs-lookup"><span data-stu-id="6330b-109">This means that it is usually used after a query has already been performed.</span></span> <span data-ttu-id="6330b-110">Metodu Load Extension lze použít k provedení dotazu, aby kontext sleduje výsledky.</span><span class="sxs-lookup"><span data-stu-id="6330b-110">The Load extension method can be used to execute a query so that the context tracks the results.</span></span> <span data-ttu-id="6330b-111">Příklad:</span><span class="sxs-lookup"><span data-stu-id="6330b-111">For example:</span></span>  

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

<span data-ttu-id="6330b-112">Pokud máme dva Blogy v databázi-' ADO.NET blog ' s BlogIdem 1 a blogem sady Visual Studio s BlogIdem 2 – můžeme očekávat následující výstup:</span><span class="sxs-lookup"><span data-stu-id="6330b-112">If we had two blogs in the database - 'ADO.NET Blog' with a BlogId of 1 and 'The Visual Studio Blog' with a BlogId of 2 - we could expect the following output:</span></span>  

```console
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

<span data-ttu-id="6330b-113">To ukazuje tři body:</span><span class="sxs-lookup"><span data-stu-id="6330b-113">This illustrates three points:</span></span>  

- <span data-ttu-id="6330b-114">Nový blog "můj nový blog" je součástí místní kolekce, i když ještě nebyl uložen do databáze.</span><span class="sxs-lookup"><span data-stu-id="6330b-114">The new blog 'My New Blog' is included in the Local collection even though it has not yet been saved to the database.</span></span> <span data-ttu-id="6330b-115">Tento blog má primární klíč nula, protože databáze ještě negenerovala skutečný klíč pro entitu.</span><span class="sxs-lookup"><span data-stu-id="6330b-115">This blog has a primary key of zero because the database has not yet generated a real key for the entity.</span></span>  
- <span data-ttu-id="6330b-116">Blog ADO.NET není zahrnutý v místní kolekci, i když je stále sledován pomocí kontextu.</span><span class="sxs-lookup"><span data-stu-id="6330b-116">The 'ADO.NET Blog' is not included in the local collection even though it is still being tracked by the context.</span></span> <span data-ttu-id="6330b-117">Důvodem je to, že jsme ho odebrali z Negenerickými a tím ho označíte jako odstraněný.</span><span class="sxs-lookup"><span data-stu-id="6330b-117">This is because we removed it from the DbSet thereby marking it as deleted.</span></span>  
- <span data-ttu-id="6330b-118">Když se Negenerickými používá k provedení dotazu, je blog označený k odstranění (blog ADO.NET), který je součástí výsledků a nový blog (můj nový blog), který ještě nebyl uložen do databáze, není zahrnutý ve výsledcích.</span><span class="sxs-lookup"><span data-stu-id="6330b-118">When DbSet is used to perform a query the blog marked for deletion (ADO.NET Blog) is included in the results and the new blog (My New Blog) that has not yet been saved to the database is not included in the results.</span></span> <span data-ttu-id="6330b-119">Důvodem je to, že Negenerickými provádí dotaz na databázi a vrácené výsledky vždycky odrážejí, co se nachází v databázi.</span><span class="sxs-lookup"><span data-stu-id="6330b-119">This is because DbSet is performing a query against the database and the results returned always reflect what is in the database.</span></span>  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a><span data-ttu-id="6330b-120">Použití místní k přidání a odebrání entit z kontextu</span><span class="sxs-lookup"><span data-stu-id="6330b-120">Using Local to add and remove entities from the context</span></span>  

<span data-ttu-id="6330b-121">Místní vlastnost v Negenerickými vrací [kolekci ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) s událostmi, které jsou zaelné tak, aby zůstaly synchronizované s obsahem kontextu.</span><span class="sxs-lookup"><span data-stu-id="6330b-121">The Local property on DbSet returns an [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) with events hooked up such that it stays in sync with the contents of the context.</span></span> <span data-ttu-id="6330b-122">To znamená, že entity je možné přidat nebo odebrat buď z místní kolekce, nebo z Negenerickými.</span><span class="sxs-lookup"><span data-stu-id="6330b-122">This means that entities can be added or removed from either the Local collection or the DbSet.</span></span> <span data-ttu-id="6330b-123">Také to znamená, že dotazy, které přinášejí nové entity do kontextu, budou mít za následek aktualizaci místní kolekce těmito entitami.</span><span class="sxs-lookup"><span data-stu-id="6330b-123">It also means that queries that bring new entities into the context will result in the Local collection being updated with those entities.</span></span> <span data-ttu-id="6330b-124">Příklad:</span><span class="sxs-lookup"><span data-stu-id="6330b-124">For example:</span></span>  

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

<span data-ttu-id="6330b-125">Za předpokladu, že máme několik příspěvků s příznakem "Entity-Framework" a "asp.net", může výstup vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="6330b-125">Assuming we had a few posts tagged with 'entity-framework' and 'asp.net' the output may look something like this:</span></span>  

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

<span data-ttu-id="6330b-126">To ukazuje tři body:</span><span class="sxs-lookup"><span data-stu-id="6330b-126">This illustrates three points:</span></span>  

- <span data-ttu-id="6330b-127">Nový příspěvek "Co je nového v EF", který byl přidán do místní kolekce, se bude sledovat v kontextu ve stavu přidáno.</span><span class="sxs-lookup"><span data-stu-id="6330b-127">The new post 'What's New in EF' that was added to the Local collection becomes tracked by the context in the Added state.</span></span> <span data-ttu-id="6330b-128">V případě volání metody SaveChanges bude proto vložena do databáze.</span><span class="sxs-lookup"><span data-stu-id="6330b-128">It will therefore be inserted into the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="6330b-129">Příspěvek, který byl odebrán z místní kolekce (Příručka pro začátečníky), je teď v kontextu označený jako odstraněný.</span><span class="sxs-lookup"><span data-stu-id="6330b-129">The post that was removed from the Local collection (EF Beginners Guide) is now marked as deleted in the context.</span></span> <span data-ttu-id="6330b-130">Z databáze proto bude při volání metody SaveChanges odstraněna.</span><span class="sxs-lookup"><span data-stu-id="6330b-130">It will therefore be deleted from the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="6330b-131">Další příspěvek (Příručka pro ASP.NET začátečník) načtený do kontextu s druhým dotazem se automaticky přidá do místní kolekce.</span><span class="sxs-lookup"><span data-stu-id="6330b-131">The additional post (ASP.NET Beginners Guide) loaded into the context with the second query is automatically added to the Local collection.</span></span>  

<span data-ttu-id="6330b-132">K tomu, abyste si poznamenali místní informace, je to, že vzhledem k tomu, že se jedná o kolekci ObservableCollection výkon, není Skvělé pro velký počet entit.</span><span class="sxs-lookup"><span data-stu-id="6330b-132">One final thing to note about Local is that because it is an ObservableCollection performance is not great for large numbers of entities.</span></span> <span data-ttu-id="6330b-133">Pokud tedy v kontextu pracujete s tisíci entit, nemusí být vhodné použít místní.</span><span class="sxs-lookup"><span data-stu-id="6330b-133">Therefore if you are dealing with thousands of entities in your context it may not be advisable to use Local.</span></span>  

## <a name="using-local-for-wpf-data-binding"></a><span data-ttu-id="6330b-134">Použití místní pro datovou vazbu WPF</span><span class="sxs-lookup"><span data-stu-id="6330b-134">Using Local for WPF data binding</span></span>  

<span data-ttu-id="6330b-135">Místní vlastnost na Negenerickými lze použít přímo pro datovou vazbu v aplikaci WPF, protože se jedná o instanci kolekci ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="6330b-135">The Local property on DbSet can be used directly for data binding in a WPF application because it is an instance of ObservableCollection.</span></span> <span data-ttu-id="6330b-136">Jak je popsáno v předchozích částech to znamená, že se automaticky synchronizuje s obsahem kontextu a obsah tohoto kontextu bude automaticky zůstat synchronizovaný s ním.</span><span class="sxs-lookup"><span data-stu-id="6330b-136">As described in the previous sections this means that it will automatically stay in sync with the contents of the context and the contents of the context will automatically stay in sync with it.</span></span> <span data-ttu-id="6330b-137">Všimněte si, že je nutné předem naplnit místní kolekci daty, aby bylo možné navazovat vazby, protože místní nikdy nezpůsobí databázový dotaz.</span><span class="sxs-lookup"><span data-stu-id="6330b-137">Note that you do need to pre-populate the Local collection with data for there to be anything to bind to since Local never causes a database query.</span></span>  

<span data-ttu-id="6330b-138">Nejedná se o vhodné místo pro úplnou ukázku datových vazeb WPF, ale klíčové prvky jsou:</span><span class="sxs-lookup"><span data-stu-id="6330b-138">This is not an appropriate place for a full WPF data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="6330b-139">Nastavení zdroje vazby</span><span class="sxs-lookup"><span data-stu-id="6330b-139">Setup a binding source</span></span>  
- <span data-ttu-id="6330b-140">Navažte vazby na místní vlastnost vaší sady.</span><span class="sxs-lookup"><span data-stu-id="6330b-140">Bind it to the Local property of your set</span></span>  
- <span data-ttu-id="6330b-141">Naplňte místní pomocí dotazu do databáze.</span><span class="sxs-lookup"><span data-stu-id="6330b-141">Populate Local using a query to the database.</span></span>  

## <a name="wpf-binding-to-navigation-properties"></a><span data-ttu-id="6330b-142">Vazba WPF na navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="6330b-142">WPF binding to navigation properties</span></span>  

<span data-ttu-id="6330b-143">Pokud provádíte datovou vazbu hlavního/podrobného data, můžete chtít svázat zobrazení podrobností s vlastností navigace jedné z entit.</span><span class="sxs-lookup"><span data-stu-id="6330b-143">If you are doing master/detail data binding you may want to bind the detail view to a navigation property of one of your entities.</span></span> <span data-ttu-id="6330b-144">Snadný způsob, jak tuto práci udělat, je použít pro navigační vlastnost kolekci ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="6330b-144">An easy way to make this work is to use an ObservableCollection for the navigation property.</span></span> <span data-ttu-id="6330b-145">Příklad:</span><span class="sxs-lookup"><span data-stu-id="6330b-145">For example:</span></span>  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a><span data-ttu-id="6330b-146">Použití místní k vyčištění entit v SaveChanges</span><span class="sxs-lookup"><span data-stu-id="6330b-146">Using Local to clean up entities in SaveChanges</span></span>  

<span data-ttu-id="6330b-147">Ve většině případů se entity odebrané z navigační vlastnosti nebudou automaticky označovat jako odstraněné v kontextu.</span><span class="sxs-lookup"><span data-stu-id="6330b-147">In most cases entities removed from a navigation property will not be automatically marked as deleted in the context.</span></span> <span data-ttu-id="6330b-148">Například odeberete-li objekt post z blogu. po volání metody SaveChanges nebude tento příspěvek automaticky odstraněn.</span><span class="sxs-lookup"><span data-stu-id="6330b-148">For example, if you remove a Post object from the Blog.Posts collection then that post will not be automatically deleted when SaveChanges is called.</span></span> <span data-ttu-id="6330b-149">Pokud ho potřebujete odstranit, možná budete muset najít tyto entity dangling a označit je jako odstraněné před voláním metody SaveChanges nebo jako součást přepsaného metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="6330b-149">If you need it to be deleted then you may need to find these dangling entities and mark them as deleted before calling SaveChanges or as part of an overridden SaveChanges.</span></span> <span data-ttu-id="6330b-150">Příklad:</span><span class="sxs-lookup"><span data-stu-id="6330b-150">For example:</span></span>  

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

<span data-ttu-id="6330b-151">Výše uvedený kód používá místní kolekci k vyhledání všech příspěvků a označení, které nemají odkaz na blog jako odstraněný.</span><span class="sxs-lookup"><span data-stu-id="6330b-151">The code above uses the Local collection to find all posts and marks any that do not have a blog reference as deleted.</span></span> <span data-ttu-id="6330b-152">Volání ToList – je požadováno, protože v opačném případě bude kolekce upravena voláním Remove během vytváření výčtu.</span><span class="sxs-lookup"><span data-stu-id="6330b-152">The ToList call is required because otherwise the collection will be modified by the Remove call while it is being enumerated.</span></span> <span data-ttu-id="6330b-153">Ve většině případů se můžete dotazovat přímo na místní vlastnost bez použití ToList – jako prvního.</span><span class="sxs-lookup"><span data-stu-id="6330b-153">In most other situations you can query directly against the Local property without using ToList first.</span></span>  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a><span data-ttu-id="6330b-154">Použití místních a ToBindingList pro datovou vazbu model Windows Forms</span><span class="sxs-lookup"><span data-stu-id="6330b-154">Using Local and ToBindingList for Windows Forms data binding</span></span>  

<span data-ttu-id="6330b-155">Model Windows Forms nepodporuje přímou datovou vazbu pomocí kolekci ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="6330b-155">Windows Forms does not support full fidelity data binding using ObservableCollection directly.</span></span> <span data-ttu-id="6330b-156">K získání všech výhod popsaných v předchozích částech však stále můžete použít místní vlastnost Negenerickými pro datovou vazbu.</span><span class="sxs-lookup"><span data-stu-id="6330b-156">However, you can still use the DbSet Local property for data binding to get all the benefits described in the previous sections.</span></span> <span data-ttu-id="6330b-157">Toho dosáhnete pomocí metody rozšíření ToBindingList, která vytvoří implementaci [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) , kterou zajišťuje místní kolekci ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="6330b-157">This is achieved through the ToBindingList extension method which creates an [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementation backed by the Local ObservableCollection.</span></span>  

<span data-ttu-id="6330b-158">Nejedná se o vhodné místo pro úplnou model Windows Forms ukázku datových vazeb, ale klíčové prvky jsou:</span><span class="sxs-lookup"><span data-stu-id="6330b-158">This is not an appropriate place for a full Windows Forms data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="6330b-159">Nastavení zdroje vazby objektu</span><span class="sxs-lookup"><span data-stu-id="6330b-159">Setup an object binding source</span></span>  
- <span data-ttu-id="6330b-160">Vytvořte jeho svázání s místní vlastností vaší sady pomocí Local. ToBindingList ().</span><span class="sxs-lookup"><span data-stu-id="6330b-160">Bind it to the Local property of your set using Local.ToBindingList()</span></span>  
- <span data-ttu-id="6330b-161">Naplnit místní pomocí dotazu do databáze</span><span class="sxs-lookup"><span data-stu-id="6330b-161">Populate Local using a query to the database</span></span>  

## <a name="getting-detailed-information-about-tracked-entities"></a><span data-ttu-id="6330b-162">Získání podrobných informací o sledovaných entitách</span><span class="sxs-lookup"><span data-stu-id="6330b-162">Getting detailed information about tracked entities</span></span>  

<span data-ttu-id="6330b-163">Mnohé z příkladů v této sérii používají metodu entry k vrácení instance DbEntityEntry pro entitu.</span><span class="sxs-lookup"><span data-stu-id="6330b-163">Many of the examples in this series use the Entry method to return a DbEntityEntry instance for an entity.</span></span> <span data-ttu-id="6330b-164">Tento objekt vstupu pak slouží jako výchozí bod pro shromažďování informací o entitě, jako je její aktuální stav, a také pro provádění operací na entitě, jako je například explicitní načtení související entity.</span><span class="sxs-lookup"><span data-stu-id="6330b-164">This entry object then acts as the starting point for gathering information about the entity such as its current state, as well as for performing operations on the entity such as explicitly loading a related entity.</span></span>  

<span data-ttu-id="6330b-165">Metody záznamů vrací objekty DbEntityEntry pro mnoho nebo všechny entity, které jsou sledovány kontextem.</span><span class="sxs-lookup"><span data-stu-id="6330b-165">The Entries methods return DbEntityEntry objects for many or all entities being tracked by the context.</span></span> <span data-ttu-id="6330b-166">Díky tomu můžete shromažďovat informace nebo provádět operace mnoha entit, a ne jenom jednu položku.</span><span class="sxs-lookup"><span data-stu-id="6330b-166">This allows you to gather information or perform operations on many entities rather than just a single entry.</span></span> <span data-ttu-id="6330b-167">Příklad:</span><span class="sxs-lookup"><span data-stu-id="6330b-167">For example:</span></span>  

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

<span data-ttu-id="6330b-168">Všimnete si, že zavádíme třídu autora a čtenáře do příkladu – obě tyto třídy implementují rozhraní IPerson.</span><span class="sxs-lookup"><span data-stu-id="6330b-168">You'll notice we are introducing a Author and Reader class into the example - both of these classes implement the IPerson interface.</span></span>  

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

<span data-ttu-id="6330b-169">Řekněme, že v databázi máme následující data:</span><span class="sxs-lookup"><span data-stu-id="6330b-169">Let's assume we have the following data in the database:</span></span>

<span data-ttu-id="6330b-170">Blog s BlogId = 1 a name = ' ADO.NET blog '</span><span class="sxs-lookup"><span data-stu-id="6330b-170">Blog with BlogId = 1 and Name = 'ADO.NET Blog'</span></span>  
<span data-ttu-id="6330b-171">Blog s BlogId = 2 a name = ' blog sady Visual Studio '</span><span class="sxs-lookup"><span data-stu-id="6330b-171">Blog with BlogId = 2 and Name = 'The Visual Studio Blog'</span></span>  
<span data-ttu-id="6330b-172">Blog s BlogId = 3 a názvem = ' .NET Framework blog '</span><span class="sxs-lookup"><span data-stu-id="6330b-172">Blog with BlogId = 3 and Name = '.NET Framework Blog'</span></span>  
<span data-ttu-id="6330b-173">Autor s AuthorId = 1 a name = ' Jan Bloggs '</span><span class="sxs-lookup"><span data-stu-id="6330b-173">Author with AuthorId = 1 and Name = 'Joe Bloggs'</span></span>  
<span data-ttu-id="6330b-174">Čtecí modul s ReaderId = 1 a name = Jan Chvojková</span><span class="sxs-lookup"><span data-stu-id="6330b-174">Reader with ReaderId = 1 and Name = 'John Doe'</span></span>  

<span data-ttu-id="6330b-175">Výstup z běhu kódu by byl:</span><span class="sxs-lookup"><span data-stu-id="6330b-175">The output from running the code would be:</span></span>  

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

<span data-ttu-id="6330b-176">Tyto příklady ilustrují několik bodů:</span><span class="sxs-lookup"><span data-stu-id="6330b-176">These examples illustrate several points:</span></span>  

- <span data-ttu-id="6330b-177">Metody záznamů vrací položky pro entity ve všech stavech, včetně odstranění.</span><span class="sxs-lookup"><span data-stu-id="6330b-177">The Entries methods return entries for entities in all states, including Deleted.</span></span> <span data-ttu-id="6330b-178">Porovnat tuto místní a vyloučí odstraněné entity.</span><span class="sxs-lookup"><span data-stu-id="6330b-178">Compare this to Local which excludes Deleted entities.</span></span>  
- <span data-ttu-id="6330b-179">Položky pro všechny typy entit jsou vráceny při použití metody neobecného typu.</span><span class="sxs-lookup"><span data-stu-id="6330b-179">Entries for all entity types are returned when the non-generic Entries method is used.</span></span> <span data-ttu-id="6330b-180">Když je metoda obecných záznamů použita, vrátí se pouze pro entity, které jsou instancemi obecného typu.</span><span class="sxs-lookup"><span data-stu-id="6330b-180">When the generic entries method is used entries are only returned for entities that are instances of the generic type.</span></span> <span data-ttu-id="6330b-181">Výše se použila k získání položek pro všechny blogy.</span><span class="sxs-lookup"><span data-stu-id="6330b-181">This was used above to get entries for all blogs.</span></span> <span data-ttu-id="6330b-182">Byl také použit k získání položek pro všechny entity, které implementují IPerson.</span><span class="sxs-lookup"><span data-stu-id="6330b-182">It was also used to get entries for all entities that implement IPerson.</span></span> <span data-ttu-id="6330b-183">To ukazuje, že obecný typ nemusí být skutečný typ entity.</span><span class="sxs-lookup"><span data-stu-id="6330b-183">This demonstrates that the generic type does not have to be an actual entity type.</span></span>  
- <span data-ttu-id="6330b-184">LINQ to Objects lze použít k filtrování vrácených výsledků.</span><span class="sxs-lookup"><span data-stu-id="6330b-184">LINQ to Objects can be used to filter the results returned.</span></span> <span data-ttu-id="6330b-185">Toto bylo použito výše pro vyhledání entit libovolného typu, pokud jsou upraveny.</span><span class="sxs-lookup"><span data-stu-id="6330b-185">This was used above to find entities of any type as long as they are modified.</span></span>  

<span data-ttu-id="6330b-186">Všimněte si, že instance DbEntityEntry vždy obsahují entitu, která není null.</span><span class="sxs-lookup"><span data-stu-id="6330b-186">Note that DbEntityEntry instances always contain a non-null Entity.</span></span> <span data-ttu-id="6330b-187">Položky vztahů a zástupné procedury nejsou reprezentovány jako instance DbEntityEntry, takže je není nutné vyfiltrovat.</span><span class="sxs-lookup"><span data-stu-id="6330b-187">Relationship entries and stub entries are not represented as DbEntityEntry instances so there is no need to filter for these.</span></span>
