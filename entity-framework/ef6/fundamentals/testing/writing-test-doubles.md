---
title: Ve Visual Basicu s vlastním testu zdvojnásobí - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
caps.latest.revision: 4
ms.openlocfilehash: 4e2511f92f9bb034ab468dd030ef238e325ce7c0
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914121"
---
# <a name="testing-with-your-own-test-doubles"></a><span data-ttu-id="cf4bc-102">Testování s čísly typu Double vlastní test</span><span class="sxs-lookup"><span data-stu-id="cf4bc-102">Testing with your own test doubles</span></span>
> [!NOTE]
> <span data-ttu-id="cf4bc-103">**EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="cf4bc-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="cf4bc-105">Při psaní testů pro vaše aplikace je často žádoucí vyhnout se dosažení databáze.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="cf4bc-106">Entity Framework umožňuje dosáhnout vytvořením kontextu – chování definované testování – která používá data v paměti.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="cf4bc-107">Možnosti pro vytvoření testu zdvojnásobí</span><span class="sxs-lookup"><span data-stu-id="cf4bc-107">Options for creating test doubles</span></span>  

<span data-ttu-id="cf4bc-108">Existují dva různé přístupy, které lze použít k vytvoření v paměti verze kontextu.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="cf4bc-109">**Vytvoření vlastního testu zdvojnásobí** – tento postup zahrnuje zápis vlastní implementaci kontextu a DbSets v paměti.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="cf4bc-110">To vám přináší značnou kontroly nad jak třídy chovat, ale může zahrnovat zápis a vlastnící přiměřené kódu.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="cf4bc-111">**Napodobování rozhraní framework použít k vytvoření testu zdvojnásobí** – využívá napodobování architekturu (například Moq) můžete mít v paměti implementace je kontext a sady pro vás vytvořili dynamicky za běhu.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of you context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="cf4bc-112">V tomto článku se bude zabývat vytváření vlastních testů double.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-112">This article will deal with creating your own test double.</span></span> <span data-ttu-id="cf4bc-113">Informace o použití napodobování framework naleznete v tématu [testování pomocí rozhraní napodobování](mocking.md).</span><span class="sxs-lookup"><span data-stu-id="cf4bc-113">For information on using a mocking framework see [Testing with a Mocking Framework](mocking.md).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="cf4bc-114">Ve Visual Basicu s EF6 předběžné verze</span><span class="sxs-lookup"><span data-stu-id="cf4bc-114">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="cf4bc-115">Je kompatibilní s EF6 kódu uvedeného v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-115">The code shown in this article is compatible with EF6.</span></span> <span data-ttu-id="cf4bc-116">Ve Visual Basicu s EF5 a starší verze najdete v části [testování s kontextem falešné](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="cf4bc-116">For testing with EF5 and earlier version see [Testing with a Fake Context](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="cf4bc-117">Omezení hodnot datového typu Double EF test v paměti</span><span class="sxs-lookup"><span data-stu-id="cf4bc-117">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="cf4bc-118">V paměti testu zdvojnásobí může být dobrým způsobem, jak poskytnout bitů vaší aplikace, které používají EF úrovně pokrytí testování částí.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-118">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="cf4bc-119">Ale při tomto postupu použijete LINQ to Objects k provádění dotazů na data v paměti.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-119">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="cf4bc-120">Výsledkem může být odlišné chování než pomocí EF pro zprostředkovatele LINQ (LINQ to Entities) ke dotazy SQL, který běží na vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-120">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="cf4bc-121">Příkladem takových rozdíl načítá související data.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-121">One example of such a difference is loading related data.</span></span> <span data-ttu-id="cf4bc-122">Pokud vytvoříte sérii blogů, které mají související příspěvky, pak při používání dat v paměti související příspěvky se vždy načteno pro každý Blog.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-122">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="cf4bc-123">Ale při spuštění pro databázi data pouze se načtou Pokud použijete metodu zahrnout.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-123">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="cf4bc-124">Z tohoto důvodu se doporučuje vždy zahrnovat určitou úroveň začátku do konce testování (navíc k testování částí) pro zajištění vaší aplikace pracuje správně proti databázi.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-124">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="cf4bc-125">Následující spolu se v tomto článku</span><span class="sxs-lookup"><span data-stu-id="cf4bc-125">Following along with this article</span></span>  

<span data-ttu-id="cf4bc-126">Tento článek obsahuje kompletní kód výpisů, které můžete zkopírovat do sady Visual Studio chcete postup sledovat, pokud chcete.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-126">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="cf4bc-127">Je nejjednodušší vytvořit **projekt testu jednotek** a budete potřebovat k cíli **rozhraní .NET Framework 4.5** dokončete oddíly, které použít operátory async.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-127">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="creating-a-context-interface"></a><span data-ttu-id="cf4bc-128">Vytvoření rozhraní pro kontext</span><span class="sxs-lookup"><span data-stu-id="cf4bc-128">Creating a context interface</span></span>  

<span data-ttu-id="cf4bc-129">Vytvoříme si prohlédnout službu, která využívá EF testování modelu.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-129">We're going to look at testing a service that makes use of an EF model.</span></span> <span data-ttu-id="cf4bc-130">Aby bylo možné nahradit náš kontext EF s verzí v paměti pro testování, budeme definovat rozhraní, že náš EF kontext (a v paměti double) bude imeplement.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-130">In order to be able to replace our EF context with an in-memory version for testing, we'll define an interface that our EF context (and it's in-memory double) will imeplement.</span></span>  

<span data-ttu-id="cf4bc-131">Na službu, kterou budeme testování dotazování a úpravy dat pomocí vlastnosti DbSet náš kontext a také volána metoda SaveChanges vložíte změny do databáze.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-131">The service we are going to test will query and modify data using the DbSet properties of our context and also call SaveChanges to push changes to the database.</span></span> <span data-ttu-id="cf4bc-132">Proto jsme zahrnutí těchto členů rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-132">So we're including these members on the interface.</span></span>  

``` csharp
using System.Data.Entity;

namespace TestingDemo
{
    public interface IBloggingContext
    {
        DbSet<Blog> Blogs { get; }
        DbSet<Post> Posts { get; }
        int SaveChanges();
    }
}
```  

## <a name="the-ef-model"></a><span data-ttu-id="cf4bc-133">EF modelu</span><span class="sxs-lookup"><span data-stu-id="cf4bc-133">The EF model</span></span>  

<span data-ttu-id="cf4bc-134">Chceme otestovat služba využívá EF modelu tvořené BloggingContext a třídy blogu a příspěvku.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-134">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="cf4bc-135">Tento kód může být vygenerováno EF designeru nebo modelu Code First.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-135">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext, IBloggingContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }
        public string Url { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }
}
```  

### <a name="implementing-the-context-interface-with-the-ef-designer"></a><span data-ttu-id="cf4bc-136">Implementace rozhraní kontextu s EF designeru</span><span class="sxs-lookup"><span data-stu-id="cf4bc-136">Implementing the context interface with the EF Designer</span></span>  

<span data-ttu-id="cf4bc-137">Všimněte si, že náš kontext implementuje rozhraní IBloggingContext.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-137">Note that our context implements the IBloggingContext interface.</span></span>  

<span data-ttu-id="cf4bc-138">Pokud používáte Code First můžete upravit kontext přímo k implementaci rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-138">If you are using Code First then you can edit your context directly to implement the interface.</span></span> <span data-ttu-id="cf4bc-139">Pokud používáte EF designeru pak bude nutné upravit šablonu T4, která generuje váš kontext.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-139">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="cf4bc-140">Otevřete \<název_modelu\>. Context.TT souboru, která je vnořená v rámci které souboru edmx, vyhledejte následující fragment kódu a přidejte rozhraní, jak je znázorněno.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-140">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the interface as shown.</span></span>  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a><span data-ttu-id="cf4bc-141">Služba má být testována</span><span class="sxs-lookup"><span data-stu-id="cf4bc-141">Service to be tested</span></span>  

<span data-ttu-id="cf4bc-142">K předvedení testování s čísly typu Double test v paměti budeme psát několik testů pro BlogService.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-142">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="cf4bc-143">Služba je schopná vytvořit nové blogy (AddBlog) a vrací všechny blogy seřazené podle názvu (GetAllBlogs).</span><span class="sxs-lookup"><span data-stu-id="cf4bc-143">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="cf4bc-144">Kromě GetAllBlogs poskytujeme také metodu, která asynchronně získá všechny blogy seřazené podle názvu (GetAllBlogsAsync).</span><span class="sxs-lookup"><span data-stu-id="cf4bc-144">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class BlogService
    {
        private IBloggingContext _context;

        public BlogService(IBloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = new Blog { Name = name, Url = url };
            _context.Blogs.Add(blog);
            _context.SaveChanges();

            return blog;
        }

        public List<Blog> GetAllBlogs()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return query.ToList();
        }

        public async Task<List<Blog>> GetAllBlogsAsync()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return await query.ToListAsync();
        }
    }
}
```  

<span data-ttu-id="cf4bc-145"><a name="creating-the-in-memory-test-doubles"/> ## Vytvoření testu v paměti zdvojnásobuje</span><span class="sxs-lookup"><span data-stu-id="cf4bc-145"><a name="creating-the-in-memory-test-doubles"/> ## Creating the in-memory test doubles</span></span>  

<span data-ttu-id="cf4bc-146">Když teď máme skutečné modelu EF a služby, můžete použít, je čas vytvořit test v paměti double, můžeme použít pro testování.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-146">Now that we have the real EF model and the service that can use it, it's time to create the in-memory test double that we can use for testing.</span></span> <span data-ttu-id="cf4bc-147">Vytvořili jsme TestContext test double pro náš kontext.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-147">We've created a TestContext test double for our context.</span></span> <span data-ttu-id="cf4bc-148">V testu zdvojnásobí, kterou dostaneme k zvolte chování chceme, aby bylo možné podporovat testy budeme ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-148">In test doubles we get to choose the behavior we want in order to support the tests we are going to run.</span></span> <span data-ttu-id="cf4bc-149">V tomto příkladu jsme právě zachycení počet pokusů, které je volána metoda SaveChanges, ale může obsahovat libovolnou logiku, je potřeba ověřit scénář, který testujete.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-149">In this example we're just capturing the number of times SaveChanges is called, but you can include whatever logic is needed to verify the scenario you are testing.</span></span>  

<span data-ttu-id="cf4bc-150">Vytvořili jsme také TestDbSet, poskytující DbSet implementace v paměti.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-150">We've also created a TestDbSet that provides an in-memory implementation of DbSet.</span></span> <span data-ttu-id="cf4bc-151">Poskytujeme kompletní čerpat pro všechny metody v DbSet (s výjimkou najít), ale potřebujete implementovat členy, které budou používat testovací scénář.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-151">We've provided a complete implemention for all the methods on DbSet (except for Find), but you only need to implement the members that your test scenario will use.</span></span>  

<span data-ttu-id="cf4bc-152">TestDbSet využívá některé infrastruktury třídy, které jsme zahrnuli zajistit, že asynchronní dotazy můžete zpracovávat.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-152">TestDbSet makes use of some other infrastructure classes that we've included to ensure that async queries can be processed.</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class TestContext : IBloggingContext
    {
        public TestContext()
        {
            this.Blogs = new TestDbSet<Blog>();
            this.Posts = new TestDbSet<Post>();
        }

        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
        public int SaveChangesCount { get; private set; }
        public int SaveChanges()
        {
            this.SaveChangesCount++;
            return 1;
        }
    }

    public class TestDbSet<TEntity> : DbSet<TEntity>, IQueryable, IEnumerable<TEntity>, IDbAsyncEnumerable<TEntity>
        where TEntity : class
    {
        ObservableCollection<TEntity> _data;
        IQueryable _query;

        public TestDbSet()
        {
            _data = new ObservableCollection<TEntity>();
            _query = _data.AsQueryable();
        }

        public override TEntity Add(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Remove(TEntity item)
        {
            _data.Remove(item);
            return item;
        }

        public override TEntity Attach(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Create()
        {
            return Activator.CreateInstance<TEntity>();
        }

        public override TDerivedEntity Create<TDerivedEntity>()
        {
            return Activator.CreateInstance<TDerivedEntity>();
        }

        public override ObservableCollection<TEntity> Local
        {
            get { return _data; }
        }

        Type IQueryable.ElementType
        {
            get { return _query.ElementType; }
        }

        Expression IQueryable.Expression
        {
            get { return _query.Expression; }
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<TEntity>(_query.Provider); }
        }

        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IEnumerator<TEntity> IEnumerable<TEntity>.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IDbAsyncEnumerator<TEntity> IDbAsyncEnumerable<TEntity>.GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<TEntity>(_data.GetEnumerator());
        }
    }

    internal class TestDbAsyncQueryProvider<TEntity> : IDbAsyncQueryProvider
    {
        private readonly IQueryProvider _inner;

        internal TestDbAsyncQueryProvider(IQueryProvider inner)
        {
            _inner = inner;
        }

        public IQueryable CreateQuery(Expression expression)
        {
            return new TestDbAsyncEnumerable<TEntity>(expression);
        }

        public IQueryable<TElement> CreateQuery<TElement>(Expression expression)
        {
            return new TestDbAsyncEnumerable<TElement>(expression);
        }

        public object Execute(Expression expression)
        {
            return _inner.Execute(expression);
        }

        public TResult Execute<TResult>(Expression expression)
        {
            return _inner.Execute<TResult>(expression);
        }

        public Task<object> ExecuteAsync(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute(expression));
        }

        public Task<TResult> ExecuteAsync<TResult>(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute<TResult>(expression));
        }
    }

    internal class TestDbAsyncEnumerable<T> : EnumerableQuery<T>, IDbAsyncEnumerable<T>, IQueryable<T>
    {
        public TestDbAsyncEnumerable(IEnumerable<T> enumerable)
            : base(enumerable)
        { }

        public TestDbAsyncEnumerable(Expression expression)
            : base(expression)
        { }

        public IDbAsyncEnumerator<T> GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<T>(this.AsEnumerable().GetEnumerator());
        }

        IDbAsyncEnumerator IDbAsyncEnumerable.GetAsyncEnumerator()
        {
            return GetAsyncEnumerator();
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<T>(this); }
        }
    }

    internal class TestDbAsyncEnumerator<T> : IDbAsyncEnumerator<T>
    {
        private readonly IEnumerator<T> _inner;

        public TestDbAsyncEnumerator(IEnumerator<T> inner)
        {
            _inner = inner;
        }

        public void Dispose()
        {
            _inner.Dispose();
        }

        public Task<bool> MoveNextAsync(CancellationToken cancellationToken)
        {
            return Task.FromResult(_inner.MoveNext());
        }

        public T Current
        {
            get { return _inner.Current; }
        }

        object IDbAsyncEnumerator.Current
        {
            get { return Current; }
        }
    }
}
```  

### <a name="implementing-find"></a><span data-ttu-id="cf4bc-153">Implementace hledání</span><span class="sxs-lookup"><span data-stu-id="cf4bc-153">Implementing Find</span></span>  

<span data-ttu-id="cf4bc-154">Metoda hledání je obtížné implementovat obecně.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-154">The Find method is difficult to implement in a generic fashion.</span></span> <span data-ttu-id="cf4bc-155">Pokud je potřeba testovat kód, který provádí použijte metodu najít, je nejjednodušší vytvořit test najít DbSet pro všechny typy entit, které potřebují podporu.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-155">If you need to test code that makes use of the Find method it is easiest to create a test DbSet for each of the entity types that need to support find.</span></span> <span data-ttu-id="cf4bc-156">Poté můžete napsat logiku k vyhledání konkrétního typu entity, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-156">You can then write logic to find that particular type of entity, as shown below.</span></span>  

``` csharp
using System.Linq;

namespace TestingDemo
{
    class TestBlogDbSet : TestDbSet<Blog>
    {
        public override Blog Find(params object[] keyValues)
        {
            var id = (int)keyValues.Single();
            return this.SingleOrDefault(b => b.BlogId == id);
        }
    }
}
```  

## <a name="writing-some-tests"></a><span data-ttu-id="cf4bc-157">Zápis některé testy</span><span class="sxs-lookup"><span data-stu-id="cf4bc-157">Writing some tests</span></span>  

<span data-ttu-id="cf4bc-158">To je všechno, co musíme udělat pro začátek testování.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-158">That’s all we need to do to start testing.</span></span> <span data-ttu-id="cf4bc-159">Následující test vytvoří TestContext a pak je služba založená na tomto kontextu.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-159">The following test creates a TestContext and then a service based on this context.</span></span> <span data-ttu-id="cf4bc-160">Služba potom slouží k vytvoření nového blogu – AddBlog metodou.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-160">The service is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="cf4bc-161">Nakonec test ověří, zda služba přidání nového blogu k vlastnosti objektu context blogy a volána metoda SaveChanges pro daný kontext.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-161">Finally, the test verifies that the service added a new Blog to the context's Blogs property and called SaveChanges on the context.</span></span>  

<span data-ttu-id="cf4bc-162">Toto je jenom pro příklad druhy věcí, které můžete otestovat s dvojitou testu v paměti a můžeme upravit logiku testu zdvojnásobí a ověřování podle svých požadavků.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-162">This is just an example of the types of things you can test with an in-memory test double and you can adjust the logic of the test doubles and the verification to meet your requirements.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var context = new TestContext();

            var service = new BlogService(context);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            Assert.AreEqual(1, context.Blogs.Count());
            Assert.AreEqual("ADO.NET Blog", context.Blogs.Single().Name);
            Assert.AreEqual("http://blogs.msdn.com/adonet", context.Blogs.Single().Url);
            Assert.AreEqual(1, context.SaveChangesCount);
        }
    }
}
```  

<span data-ttu-id="cf4bc-163">Tady je další příklad testu – tentokrát ten, který do searche zadá dotaz.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-163">Here is another example of a test - this time one that performs a query.</span></span> <span data-ttu-id="cf4bc-164">Test začíná tím, že vytvoříte kontextu testu s daty v jeho Blogovém vlastnost – Všimněte si, že data nejsou v abecedním pořadí.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-164">The test starts by creating a test context with some data in its Blog property - note that the data is not in alphabetical order.</span></span> <span data-ttu-id="cf4bc-165">Pak můžeme vytvořit BlogService na základě našich kontextu testu a ujistěte se, že data, která jsme získat zpět z GetAllBlogs je seřazen podle názvu.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-165">We can then create a BlogService based on our test context and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

<span data-ttu-id="cf4bc-166">A konečně, budeme psát jeden další test, který využívá naše asynchronní metody a zkontrolujte, že jsme součástí infrastruktury asynchronní [TestDbSet](#creating-the-in-memory-test-doubles) funguje.</span><span class="sxs-lookup"><span data-stu-id="cf4bc-166">Finally, we'll write one more test that uses our async method to ensure that the async infrastructure we included in [TestDbSet](#creating-the-in-memory-test-doubles) is working.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    [TestClass]
    public class AsyncQueryTests
    {
        [TestMethod]
        public async Task GetAllBlogsAsync_orders_by_name()
        {
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
