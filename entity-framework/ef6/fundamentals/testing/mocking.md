---
title: Ve Visual Basicu s napodobování framework – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: abb915f2df5645b3838a659dafc59a2b499b4ee2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998210"
---
# <a name="testing-with-a-mocking-framework"></a><span data-ttu-id="71b10-102">Testování s napodobování framework</span><span class="sxs-lookup"><span data-stu-id="71b10-102">Testing with a mocking framework</span></span>
> [!NOTE]
> <span data-ttu-id="71b10-103">**EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="71b10-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="71b10-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="71b10-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="71b10-105">Při psaní testů pro vaše aplikace je často žádoucí vyhnout se dosažení databáze.</span><span class="sxs-lookup"><span data-stu-id="71b10-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="71b10-106">Entity Framework umožňuje dosáhnout vytvořením kontextu – chování definované testování – která používá data v paměti.</span><span class="sxs-lookup"><span data-stu-id="71b10-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="71b10-107">Možnosti pro vytvoření testu zdvojnásobí</span><span class="sxs-lookup"><span data-stu-id="71b10-107">Options for creating test doubles</span></span>  

<span data-ttu-id="71b10-108">Existují dva různé přístupy, které lze použít k vytvoření v paměti verze kontextu.</span><span class="sxs-lookup"><span data-stu-id="71b10-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="71b10-109">**Vytvoření vlastního testu zdvojnásobí** – tento postup zahrnuje zápis vlastní implementaci kontextu a DbSets v paměti.</span><span class="sxs-lookup"><span data-stu-id="71b10-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="71b10-110">To vám přináší značnou kontroly nad jak třídy chovat, ale může zahrnovat zápis a vlastnící přiměřené kódu.</span><span class="sxs-lookup"><span data-stu-id="71b10-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="71b10-111">**Napodobování rozhraní framework použít k vytvoření testu zdvojnásobí** – využívá napodobování architekturu (například Moq) můžete mít v paměti implementace je kontext a sady pro vás vytvořili dynamicky za běhu.</span><span class="sxs-lookup"><span data-stu-id="71b10-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of you context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="71b10-112">V tomto článku se postará o s využitím napodobování architektury.</span><span class="sxs-lookup"><span data-stu-id="71b10-112">This article will deal with using a mocking framework.</span></span> <span data-ttu-id="71b10-113">Vytvoření vlastního testu zdvojnásobí naleznete v tématu [testování se váš vlastní testování hodnot datového typu Double](writing-test-doubles.md).</span><span class="sxs-lookup"><span data-stu-id="71b10-113">For creating your own test doubles see [Testing with Your Own Test Doubles](writing-test-doubles.md).</span></span>  

<span data-ttu-id="71b10-114">Demonstruje použití EF s napodobování framework budeme používat Moq.</span><span class="sxs-lookup"><span data-stu-id="71b10-114">To demonstrate using EF with a mocking framework we are going to use Moq.</span></span> <span data-ttu-id="71b10-115">Nejjednodušší způsob, jak získat Moq je instalace [Moq balíčku od Nugetu](http://nuget.org/packages/Moq/).</span><span class="sxs-lookup"><span data-stu-id="71b10-115">The easiest way to get Moq is to install the [Moq package from NuGet](http://nuget.org/packages/Moq/).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="71b10-116">Ve Visual Basicu s EF6 předběžné verze</span><span class="sxs-lookup"><span data-stu-id="71b10-116">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="71b10-117">Scénáře uvedené v tomto článku je závislá na několik změn, které jsme provedli DbSet v EF6.</span><span class="sxs-lookup"><span data-stu-id="71b10-117">The scenario shown in this article is dependent on some changes we made to DbSet in EF6.</span></span> <span data-ttu-id="71b10-118">Ve Visual Basicu s EF5 a starší verze najdete v části [testování s kontextem falešné](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="71b10-118">For testing with EF5 and earlier version see [Testing with a Fake Context](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="71b10-119">Omezení hodnot datového typu Double EF test v paměti</span><span class="sxs-lookup"><span data-stu-id="71b10-119">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="71b10-120">V paměti testu zdvojnásobí může být dobrým způsobem, jak poskytnout bitů vaší aplikace, které používají EF úrovně pokrytí testování částí.</span><span class="sxs-lookup"><span data-stu-id="71b10-120">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="71b10-121">Ale při tomto postupu použijete LINQ to Objects k provádění dotazů na data v paměti.</span><span class="sxs-lookup"><span data-stu-id="71b10-121">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="71b10-122">Výsledkem může být odlišné chování než pomocí EF pro zprostředkovatele LINQ (LINQ to Entities) ke dotazy SQL, který běží na vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="71b10-122">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="71b10-123">Příkladem takových rozdíl načítá související data.</span><span class="sxs-lookup"><span data-stu-id="71b10-123">One example of such a difference is loading related data.</span></span> <span data-ttu-id="71b10-124">Pokud vytvoříte sérii blogů, které mají související příspěvky, pak při používání dat v paměti související příspěvky se vždy načteno pro každý Blog.</span><span class="sxs-lookup"><span data-stu-id="71b10-124">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="71b10-125">Ale při spuštění pro databázi data pouze se načtou Pokud použijete metodu zahrnout.</span><span class="sxs-lookup"><span data-stu-id="71b10-125">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="71b10-126">Z tohoto důvodu se doporučuje vždy zahrnovat určitou úroveň začátku do konce testování (navíc k testování částí) pro zajištění vaší aplikace pracuje správně proti databázi.</span><span class="sxs-lookup"><span data-stu-id="71b10-126">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="71b10-127">Následující spolu se v tomto článku</span><span class="sxs-lookup"><span data-stu-id="71b10-127">Following along with this article</span></span>  

<span data-ttu-id="71b10-128">Tento článek obsahuje kompletní kód výpisů, které můžete zkopírovat do sady Visual Studio chcete postup sledovat, pokud chcete.</span><span class="sxs-lookup"><span data-stu-id="71b10-128">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="71b10-129">Je nejjednodušší vytvořit **projekt testu jednotek** a budete potřebovat k cíli **rozhraní .NET Framework 4.5** dokončete oddíly, které použít operátory async.</span><span class="sxs-lookup"><span data-stu-id="71b10-129">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="the-ef-model"></a><span data-ttu-id="71b10-130">EF modelu</span><span class="sxs-lookup"><span data-stu-id="71b10-130">The EF model</span></span>  

<span data-ttu-id="71b10-131">Chceme otestovat služba využívá EF modelu tvořené BloggingContext a třídy blogu a příspěvku.</span><span class="sxs-lookup"><span data-stu-id="71b10-131">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="71b10-132">Tento kód může být vygenerováno EF designeru nebo modelu Code First.</span><span class="sxs-lookup"><span data-stu-id="71b10-132">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext
    {
        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }
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

### <a name="virtual-dbset-properties-with-ef-designer"></a><span data-ttu-id="71b10-133">Vlastnosti virtuální DbSet s EF designeru</span><span class="sxs-lookup"><span data-stu-id="71b10-133">Virtual DbSet properties with EF Designer</span></span>  

<span data-ttu-id="71b10-134">Všimněte si, že vlastnosti DbSet na kontextu, jsou označeny jako virtuální.</span><span class="sxs-lookup"><span data-stu-id="71b10-134">Note that the DbSet properties on the context are marked as virtual.</span></span> <span data-ttu-id="71b10-135">To vám umožní napodobování framework odvodit z našich kontextu a přepsání těchto vlastností s imitaci implementací.</span><span class="sxs-lookup"><span data-stu-id="71b10-135">This will allow the mocking framework to derive from our context and overriding these properties with a mocked implementation.</span></span>  

<span data-ttu-id="71b10-136">Pokud používáte Code First, pak tříd můžete upravovat přímo.</span><span class="sxs-lookup"><span data-stu-id="71b10-136">If you are using Code First then you can edit your classes directly.</span></span> <span data-ttu-id="71b10-137">Pokud používáte EF designeru pak bude nutné upravit šablonu T4, která generuje váš kontext.</span><span class="sxs-lookup"><span data-stu-id="71b10-137">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="71b10-138">Otevřete \<název_modelu\>. Context.TT souboru, která je vnořená v rámci které souboru edmx, najít následující fragment kódu a přidejte do virtual – klíčové slovo, jak je znázorněno.</span><span class="sxs-lookup"><span data-stu-id="71b10-138">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the virtual keyword as shown.</span></span>  

``` csharp
public string DbSet(EntitySet entitySet)
{
    return string.Format(
        CultureInfo.InvariantCulture,
        "{0} virtual DbSet\<{1}> {2} {{ get; set; }}",
        Accessibility.ForReadOnlyProperty(entitySet),
        _typeMapper.GetTypeName(entitySet.ElementType),
        _code.Escape(entitySet));
}
```  

## <a name="service-to-be-tested"></a><span data-ttu-id="71b10-139">Služba má být testována</span><span class="sxs-lookup"><span data-stu-id="71b10-139">Service to be tested</span></span>  

<span data-ttu-id="71b10-140">K předvedení testování s čísly typu Double test v paměti budeme psát několik testů pro BlogService.</span><span class="sxs-lookup"><span data-stu-id="71b10-140">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="71b10-141">Služba je schopná vytvořit nové blogy (AddBlog) a vrací všechny blogy seřazené podle názvu (GetAllBlogs).</span><span class="sxs-lookup"><span data-stu-id="71b10-141">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="71b10-142">Kromě GetAllBlogs poskytujeme také metodu, která asynchronně získá všechny blogy seřazené podle názvu (GetAllBlogsAsync).</span><span class="sxs-lookup"><span data-stu-id="71b10-142">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class BlogService
    {
        private BloggingContext _context;

        public BlogService(BloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = _context.Blogs.Add(new Blog { Name = name, Url = url });
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

## <a name="testing-non-query-scenarios"></a><span data-ttu-id="71b10-143">Scénáře testování bez dotazů</span><span class="sxs-lookup"><span data-stu-id="71b10-143">Testing non-query scenarios</span></span>  

<span data-ttu-id="71b10-144">To je všechno, co musíme udělat pro začátek testování metody bez dotazů.</span><span class="sxs-lookup"><span data-stu-id="71b10-144">That’s all we need to do to start testing non-query methods.</span></span> <span data-ttu-id="71b10-145">Následující testovací Moq používá k vytvoření kontextu.</span><span class="sxs-lookup"><span data-stu-id="71b10-145">The following test uses Moq to create a context.</span></span> <span data-ttu-id="71b10-146">Pak vytvoří DbSet\<blogu\> a sváže má být vrácena z vlastnosti blogy kontextu.</span><span class="sxs-lookup"><span data-stu-id="71b10-146">It then creates a DbSet\<Blog\> and wires it up to be returned from the context’s Blogs property.</span></span> <span data-ttu-id="71b10-147">V dalším kroku kontext slouží k vytvoření nové BlogService, které se pak použije k vytvoření nového blogu – AddBlog metodou.</span><span class="sxs-lookup"><span data-stu-id="71b10-147">Next, the context is used to create a new BlogService which is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="71b10-148">Nakonec test ověří, zda služba přidání nového blogu a volána metoda SaveChanges pro daný kontext.</span><span class="sxs-lookup"><span data-stu-id="71b10-148">Finally, the test verifies that the service added a new Blog and called SaveChanges on the context.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Data.Entity;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var mockSet = new Mock<DbSet<Blog>>();

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(m => m.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            mockSet.Verify(m => m.Add(It.IsAny<Blog>()), Times.Once());
            mockContext.Verify(m => m.SaveChanges(), Times.Once());
        }
    }
}
```  

## <a name="testing-query-scenarios"></a><span data-ttu-id="71b10-149">Scénáře testování dotazů</span><span class="sxs-lookup"><span data-stu-id="71b10-149">Testing query scenarios</span></span>  

<span data-ttu-id="71b10-150">Aby bylo možné provádět dotazy proti testovacím DbSet double musíme nastavit implementace IQueryable.</span><span class="sxs-lookup"><span data-stu-id="71b10-150">In order to be able to execute queries against our DbSet test double we need to setup an implementation of IQueryable.</span></span> <span data-ttu-id="71b10-151">Prvním krokem je vytvoření některá data v paměti – používáme seznam\<blogu\>.</span><span class="sxs-lookup"><span data-stu-id="71b10-151">The first step is to create some in-memory data – we’re using a List\<Blog\>.</span></span> <span data-ttu-id="71b10-152">V dalším kroku vytvoříme kontextu a DBSet\<blogu\> potom nastavit IQueryable implementace DbSet – právě jste delegováno na poskytovateli LINQ to Objects, která funguje s seznamu\<T\>.</span><span class="sxs-lookup"><span data-stu-id="71b10-152">Next, we create a context and DBSet\<Blog\> then wire up the IQueryable implementation for the DbSet – they’re just delegating to the LINQ to Objects provider that works with List\<T\>.</span></span>  

<span data-ttu-id="71b10-153">Pak můžeme vytvořit BlogService podle našich testu zdvojnásobí a ujistěte se, že data, která jsme získat zpět z GetAllBlogs je seřazen podle názvu.</span><span class="sxs-lookup"><span data-stu-id="71b10-153">We can then create a BlogService based on our test doubles and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Provider).Returns(data.Provider);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(0 => data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

### <a name="testing-with-async-queries"></a><span data-ttu-id="71b10-154">Testování s asynchronní dotazy</span><span class="sxs-lookup"><span data-stu-id="71b10-154">Testing with async queries</span></span>

<span data-ttu-id="71b10-155">Entity Framework 6 zavedl sadu rozšiřujících metod, které slouží k asynchronně spustí dotaz.</span><span class="sxs-lookup"><span data-stu-id="71b10-155">Entity Framework 6 introduced a set of extension methods that can be used to asynchronously execute a query.</span></span> <span data-ttu-id="71b10-156">Příklady těchto metod: ToListAsync FirstAsync, ForEachAsync, atd.</span><span class="sxs-lookup"><span data-stu-id="71b10-156">Examples of these methods include ToListAsync, FirstAsync, ForEachAsync, etc.</span></span>  

<span data-ttu-id="71b10-157">Protože Entity Framework dotazy využívají LINQ, rozšiřující metody jsou definovány na IQueryable a IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="71b10-157">Because Entity Framework queries make use of LINQ, the extension methods are defined on IQueryable and IEnumerable.</span></span> <span data-ttu-id="71b10-158">Ale protože jsou určeny pouze pro použití s Entity Framework můžete obdržet následující chybu při pokusu o jejich použití v dotazu LINQ, který není dotaz rozhraní Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="71b10-158">However, because they are only designed to be used with Entity Framework you may receive the following error if you try to use them on a LINQ query that isn’t an Entity Framework query:</span></span>

> <span data-ttu-id="71b10-159">Zdroj IQueryable neimplementuje IDbAsyncEnumerable{0}.</span><span class="sxs-lookup"><span data-stu-id="71b10-159">The source IQueryable doesn't implement IDbAsyncEnumerable{0}.</span></span> <span data-ttu-id="71b10-160">Pouze zdroje, které implementují IDbAsyncEnumerable lze použít pro asynchronní operace Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="71b10-160">Only sources that implement IDbAsyncEnumerable can be used for Entity Framework asynchronous operations.</span></span> <span data-ttu-id="71b10-161">Další podrobnosti najdete v tématu [ http://go.microsoft.com/fwlink/?LinkId=287068 ](http://go.microsoft.com/fwlink/?LinkId=287068).</span><span class="sxs-lookup"><span data-stu-id="71b10-161">For more details see [http://go.microsoft.com/fwlink/?LinkId=287068](http://go.microsoft.com/fwlink/?LinkId=287068).</span></span>  

<span data-ttu-id="71b10-162">Zatímco asynchronní metody se podporují jenom při spouštění dotazu EF, můžete chtít použít při spuštění proti v paměti testování double DbSet v testu jednotek.</span><span class="sxs-lookup"><span data-stu-id="71b10-162">Whilst the async methods are only supported when running against an EF query, you may want to use them in your unit test when running against an in-memory test double of a DbSet.</span></span>  

<span data-ttu-id="71b10-163">Pokud chcete používat asynchronní metody potřebujeme vytvořit DbAsyncQueryProvider v paměti pro zpracování asynchronního dotazu.</span><span class="sxs-lookup"><span data-stu-id="71b10-163">In order to use the async methods we need to create an in-memory DbAsyncQueryProvider to process the async query.</span></span> <span data-ttu-id="71b10-164">Zatímco by bylo možné nastavit poskytovatele dotazů pomocí Moq, je snazší vytvářet double provádění testů v kódu.</span><span class="sxs-lookup"><span data-stu-id="71b10-164">Whilst it would be possible to setup a query provider using Moq, it is much easier to create a test double implementation in code.</span></span> <span data-ttu-id="71b10-165">Kód pro tuto implementaci vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="71b10-165">The code for this implementation is as follows:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
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

<span data-ttu-id="71b10-166">Když teď máme poskytovatele asynchronního dotazu jsme pro naši novou metodu GetAllBlogsAsync napsat Jednotkový test.</span><span class="sxs-lookup"><span data-stu-id="71b10-166">Now that we have an async query provider we can write a unit test for our new GetAllBlogsAsync method.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
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

            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IDbAsyncEnumerable<Blog>>()
                .Setup(m => m.GetAsyncEnumerator())
                .Returns(new TestDbAsyncEnumerator<Blog>(data.GetEnumerator()));

            mockSet.As<IQueryable<Blog>>()
                .Setup(m => m.Provider)
                .Returns(new TestDbAsyncQueryProvider<Blog>(data.Provider));

            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
