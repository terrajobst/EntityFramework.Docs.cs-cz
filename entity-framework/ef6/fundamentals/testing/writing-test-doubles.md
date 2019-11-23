---
title: Testování s vlastním testem dvakrát – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: 3d8933fb5e17f8c01f3971495a1fcdb5b8cfab57
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2019
ms.locfileid: "72446023"
---
# <a name="testing-with-your-own-test-doubles"></a><span data-ttu-id="5f128-102">Testování s vlastními dvojitými testy</span><span class="sxs-lookup"><span data-stu-id="5f128-102">Testing with your own test doubles</span></span>
> [!NOTE]
> <span data-ttu-id="5f128-103">**EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="5f128-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="5f128-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="5f128-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="5f128-105">Při psaní testů pro aplikaci je často žádoucí vyhnout se tomu, aby se databáze mohla zacházet.</span><span class="sxs-lookup"><span data-stu-id="5f128-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="5f128-106">Entity Framework vám umožňuje dosáhnout toho, že vytvoří kontext – s chováním definovanými testy, které využívá data v paměti.</span><span class="sxs-lookup"><span data-stu-id="5f128-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="5f128-107">Možnosti pro vytváření dvojitých hodnot testu</span><span class="sxs-lookup"><span data-stu-id="5f128-107">Options for creating test doubles</span></span>  

<span data-ttu-id="5f128-108">Existují dva různé přístupy, které lze použít k vytvoření verze vašeho kontextu v paměti.</span><span class="sxs-lookup"><span data-stu-id="5f128-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="5f128-109">**Vytvořte si vlastní test Doubles** – tento přístup zahrnuje zápis vlastní implementace v paměti vašeho kontextu a DbSets.</span><span class="sxs-lookup"><span data-stu-id="5f128-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="5f128-110">Díky tomu máte spoustu kontroly nad tím, jak se třídy chovají, ale mohou zahrnovat psaní a vlastnictví přiměřeného kódu.</span><span class="sxs-lookup"><span data-stu-id="5f128-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="5f128-111">**Použijte napodobnou architekturu k vytvoření dvojitých testů** – pomocí napodobení rozhraní (například MOQ), můžete mít implementace kontextu v paměti a sady se vytvoří dynamicky za běhu za vás.</span><span class="sxs-lookup"><span data-stu-id="5f128-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of you context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="5f128-112">Tento článek se zabývat vytvářením vlastního testu dvakrát.</span><span class="sxs-lookup"><span data-stu-id="5f128-112">This article will deal with creating your own test double.</span></span> <span data-ttu-id="5f128-113">Informace o použití rozhraní pro návrhy viz testování s napodobnou [architekturou](mocking.md).</span><span class="sxs-lookup"><span data-stu-id="5f128-113">For information on using a mocking framework see [Testing with a Mocking Framework](mocking.md).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="5f128-114">Testování s EF6 verzemi</span><span class="sxs-lookup"><span data-stu-id="5f128-114">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="5f128-115">Kód uvedený v tomto článku je kompatibilní s EF6.</span><span class="sxs-lookup"><span data-stu-id="5f128-115">The code shown in this article is compatible with EF6.</span></span> <span data-ttu-id="5f128-116">Pro testování pomocí EF5 a starší verze se podívejte na [testování s falešným kontextem](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="5f128-116">For testing with EF5 and earlier version see [Testing with a Fake Context](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="5f128-117">Omezení pro dvojité testy v paměti EF</span><span class="sxs-lookup"><span data-stu-id="5f128-117">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="5f128-118">Dvojitá přesnost testu paměti může být dobrým způsobem, jak poskytnout jednotkové pokrytí na úrovni testování bitů vaší aplikace, které používají EF.</span><span class="sxs-lookup"><span data-stu-id="5f128-118">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="5f128-119">Nicméně když to uděláte, budete používat LINQ to Objects k provádění dotazů na data v paměti.</span><span class="sxs-lookup"><span data-stu-id="5f128-119">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="5f128-120">Výsledkem může být odlišné chování než použití zprostředkovatele LINQ (LINQ to Entities) EF k překladu dotazů na SQL, které se spouští proti vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="5f128-120">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="5f128-121">Jedním z příkladů takového rozdílu je načítání souvisejících dat.</span><span class="sxs-lookup"><span data-stu-id="5f128-121">One example of such a difference is loading related data.</span></span> <span data-ttu-id="5f128-122">Pokud vytvoříte řadu blogů, které mají všechny související příspěvky, pak při použití dat v paměti budou související příspěvky vždy načteny pro každý blog.</span><span class="sxs-lookup"><span data-stu-id="5f128-122">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="5f128-123">Při spuštění na databázi však budou data načtena pouze v případě, že použijete metodu include.</span><span class="sxs-lookup"><span data-stu-id="5f128-123">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="5f128-124">Z tohoto důvodu se doporučuje vždy zahrnout určitou úroveň komplexního testování (kromě testů jednotek), abyste zajistili správné fungování aplikace proti databázi.</span><span class="sxs-lookup"><span data-stu-id="5f128-124">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="5f128-125">Následující článek spolu s tímto článkem</span><span class="sxs-lookup"><span data-stu-id="5f128-125">Following along with this article</span></span>  

<span data-ttu-id="5f128-126">Tento článek obsahuje kompletní výpisy kódu, které můžete zkopírovat do sady Visual Studio, abyste mohli postupovat podle toho, co chcete.</span><span class="sxs-lookup"><span data-stu-id="5f128-126">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="5f128-127">Je nejjednodušší vytvořit **projekt testování částí** a vy budete muset cílit **.NET Framework 4,5** , abyste dokončili oddíly, které používají async.</span><span class="sxs-lookup"><span data-stu-id="5f128-127">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="creating-a-context-interface"></a><span data-ttu-id="5f128-128">Vytvoření kontextu rozhraní</span><span class="sxs-lookup"><span data-stu-id="5f128-128">Creating a context interface</span></span>  

<span data-ttu-id="5f128-129">Budeme se pohlížet na testování služby, která využívá model EF.</span><span class="sxs-lookup"><span data-stu-id="5f128-129">We're going to look at testing a service that makes use of an EF model.</span></span> <span data-ttu-id="5f128-130">Aby bylo možné nahradit náš kontext EF pomocí verze v paměti pro testování, definujeme rozhraní, které náš kontext EF (a Dvojitá paměť v paměti) provede implementaci.</span><span class="sxs-lookup"><span data-stu-id="5f128-130">In order to be able to replace our EF context with an in-memory version for testing, we'll define an interface that our EF context (and it's in-memory double) will implement.</span></span>

<span data-ttu-id="5f128-131">Služba, kterou budeme testovat, bude dotazovat se na data a upravovat je pomocí vlastností Negenerickými našeho kontextu a volat metodu SaveChanges a doručovat změny do databáze.</span><span class="sxs-lookup"><span data-stu-id="5f128-131">The service we are going to test will query and modify data using the DbSet properties of our context and also call SaveChanges to push changes to the database.</span></span> <span data-ttu-id="5f128-132">Proto do rozhraní patříme tyto členy.</span><span class="sxs-lookup"><span data-stu-id="5f128-132">So we're including these members on the interface.</span></span>  

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

## <a name="the-ef-model"></a><span data-ttu-id="5f128-133">Model EF</span><span class="sxs-lookup"><span data-stu-id="5f128-133">The EF model</span></span>  

<span data-ttu-id="5f128-134">Služba, kterou budeme testovat, používá model EF, který se skládá z BloggingContext a třídy blog a post.</span><span class="sxs-lookup"><span data-stu-id="5f128-134">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="5f128-135">Tento kód byl pravděpodobně vygenerován návrhářem EF nebo se jedná o Code First model.</span><span class="sxs-lookup"><span data-stu-id="5f128-135">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a><span data-ttu-id="5f128-136">Implementace kontextu rozhraní s návrhářem EF</span><span class="sxs-lookup"><span data-stu-id="5f128-136">Implementing the context interface with the EF Designer</span></span>  

<span data-ttu-id="5f128-137">Všimněte si, že náš kontext implementuje rozhraní IBloggingContext.</span><span class="sxs-lookup"><span data-stu-id="5f128-137">Note that our context implements the IBloggingContext interface.</span></span>  

<span data-ttu-id="5f128-138">Pokud používáte Code First pak můžete upravit kontext přímo k implementaci rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5f128-138">If you are using Code First then you can edit your context directly to implement the interface.</span></span> <span data-ttu-id="5f128-139">Pokud používáte návrháře EF, budete muset upravit šablonu T4, která generuje váš kontext.</span><span class="sxs-lookup"><span data-stu-id="5f128-139">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="5f128-140">Otevřete \<model_name\>. Soubor Context.tt, který je vnořen do souboru EDMX, vyhledejte následující fragment kódu a přidejte ho do rozhraní, jak je znázorněno na obrázku.</span><span class="sxs-lookup"><span data-stu-id="5f128-140">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the interface as shown.</span></span>  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a><span data-ttu-id="5f128-141">Služba, která se má testovat</span><span class="sxs-lookup"><span data-stu-id="5f128-141">Service to be tested</span></span>  

<span data-ttu-id="5f128-142">K předvedení testování s dvojitými pokusy o testování v paměti budeme psát několik testů pro BlogService.</span><span class="sxs-lookup"><span data-stu-id="5f128-142">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="5f128-143">Služba umožňuje vytvářet nové Blogy (AddBlog) a vracet všechny Blogy seřazené podle názvu (GetAllBlogs).</span><span class="sxs-lookup"><span data-stu-id="5f128-143">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="5f128-144">Kromě GetAllBlogs jsme také poskytli metodu, která asynchronně získá všechny Blogy seřazené podle názvu (GetAllBlogsAsync).</span><span class="sxs-lookup"><span data-stu-id="5f128-144">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

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

## <a name="creating-the-in-memory-test-doubles"></a><span data-ttu-id="5f128-145">Vytváření dvojitých hodnot testu v paměti</span><span class="sxs-lookup"><span data-stu-id="5f128-145">Creating the in-memory test doubles</span></span>  

<span data-ttu-id="5f128-146">Teď, když máme skutečný model EF a službu, která ho může používat, je čas vytvořit zdvojnásobení testu v paměti, které můžeme použít pro testování.</span><span class="sxs-lookup"><span data-stu-id="5f128-146">Now that we have the real EF model and the service that can use it, it's time to create the in-memory test double that we can use for testing.</span></span> <span data-ttu-id="5f128-147">Pro náš kontext jsme vytvořili TestContext test Double.</span><span class="sxs-lookup"><span data-stu-id="5f128-147">We've created a TestContext test double for our context.</span></span> <span data-ttu-id="5f128-148">V testu zdvojnásobme si, jak zvolit chování, které chceme, aby bylo možné podporovat testy, které budeme spouštět.</span><span class="sxs-lookup"><span data-stu-id="5f128-148">In test doubles we get to choose the behavior we want in order to support the tests we are going to run.</span></span> <span data-ttu-id="5f128-149">V tomto příkladu právě zachycujete počet volání metody SaveChanges, ale můžete zahrnout jakoukoli logiku potřebnou k ověření scénáře, který testujete.</span><span class="sxs-lookup"><span data-stu-id="5f128-149">In this example we're just capturing the number of times SaveChanges is called, but you can include whatever logic is needed to verify the scenario you are testing.</span></span>  

<span data-ttu-id="5f128-150">Vytvořili jsme také TestDbSet, který poskytuje implementaci Negenerickými v paměti.</span><span class="sxs-lookup"><span data-stu-id="5f128-150">We've also created a TestDbSet that provides an in-memory implementation of DbSet.</span></span> <span data-ttu-id="5f128-151">K dispozici je kompletní implementace všech metod na Negenerickými (s výjimkou hledání), ale stačí pouze implementovat členy, které bude váš testovací scénář používat.</span><span class="sxs-lookup"><span data-stu-id="5f128-151">We've provided a complete implemention for all the methods on DbSet (except for Find), but you only need to implement the members that your test scenario will use.</span></span>  

<span data-ttu-id="5f128-152">TestDbSet využívá některé jiné třídy infrastruktury, které jsme zahrnuli k zajištění toho, aby bylo možné zpracovávat asynchronní dotazy.</span><span class="sxs-lookup"><span data-stu-id="5f128-152">TestDbSet makes use of some other infrastructure classes that we've included to ensure that async queries can be processed.</span></span>  

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

### <a name="implementing-find"></a><span data-ttu-id="5f128-153">Implementace Find</span><span class="sxs-lookup"><span data-stu-id="5f128-153">Implementing Find</span></span>  

<span data-ttu-id="5f128-154">Metodu Find je obtížné implementovat obecným způsobem.</span><span class="sxs-lookup"><span data-stu-id="5f128-154">The Find method is difficult to implement in a generic fashion.</span></span> <span data-ttu-id="5f128-155">Pokud potřebujete testovat kód, který využívá metodu Find, je nejjednodušší vytvořit Negenerickými testu pro každý typ entity, který musí podporovat hledání.</span><span class="sxs-lookup"><span data-stu-id="5f128-155">If you need to test code that makes use of the Find method it is easiest to create a test DbSet for each of the entity types that need to support find.</span></span> <span data-ttu-id="5f128-156">Pak můžete napsat logiku k vyhledání konkrétního typu entity, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="5f128-156">You can then write logic to find that particular type of entity, as shown below.</span></span>  

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

## <a name="writing-some-tests"></a><span data-ttu-id="5f128-157">Zápis některých testů</span><span class="sxs-lookup"><span data-stu-id="5f128-157">Writing some tests</span></span>  

<span data-ttu-id="5f128-158">To je všechno, co musíme udělat k zahájení testování.</span><span class="sxs-lookup"><span data-stu-id="5f128-158">That’s all we need to do to start testing.</span></span> <span data-ttu-id="5f128-159">Následující test vytvoří TestContext a pak službu na základě tohoto kontextu.</span><span class="sxs-lookup"><span data-stu-id="5f128-159">The following test creates a TestContext and then a service based on this context.</span></span> <span data-ttu-id="5f128-160">Služba se pak použije k vytvoření nového blogu – pomocí metody AddBlog.</span><span class="sxs-lookup"><span data-stu-id="5f128-160">The service is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="5f128-161">Nakonec test ověří, že služba přidala nový blog do vlastnosti Blogy daného kontextu a v kontextu byla volána metoda SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="5f128-161">Finally, the test verifies that the service added a new Blog to the context's Blogs property and called SaveChanges on the context.</span></span>  

<span data-ttu-id="5f128-162">Toto je pouze příklad typů položek, které můžete testovat pomocí zdvojnásobení testu v paměti, a můžete upravit logiku podvojení testu a ověření, aby splňovalo vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="5f128-162">This is just an example of the types of things you can test with an in-memory test double and you can adjust the logic of the test doubles and the verification to meet your requirements.</span></span>  

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

<span data-ttu-id="5f128-163">Tady je další příklad testu – tentokrát, který provádí dotaz.</span><span class="sxs-lookup"><span data-stu-id="5f128-163">Here is another example of a test - this time one that performs a query.</span></span> <span data-ttu-id="5f128-164">Test začíná vytvořením kontextu testu s některými daty ve vlastnosti blogu – Všimněte si, že data nejsou v abecedním pořadí.</span><span class="sxs-lookup"><span data-stu-id="5f128-164">The test starts by creating a test context with some data in its Blog property - note that the data is not in alphabetical order.</span></span> <span data-ttu-id="5f128-165">Na základě našeho kontextu testování můžeme pak vytvořit BlogService a zajistit, aby se data, která vracíme z GetAllBlogs, objednala podle názvu.</span><span class="sxs-lookup"><span data-stu-id="5f128-165">We can then create a BlogService based on our test context and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

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

<span data-ttu-id="5f128-166">Nakonec zapíšeme ještě jeden test, který používá naši asynchronní metodu k zajištění toho, že asynchronní infrastruktura, kterou jsme zahrnuli do [TestDbSet](#creating-the-in-memory-test-doubles) , funguje.</span><span class="sxs-lookup"><span data-stu-id="5f128-166">Finally, we'll write one more test that uses our async method to ensure that the async infrastructure we included in [TestDbSet](#creating-the-in-memory-test-doubles) is working.</span></span>  

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
