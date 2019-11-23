---
title: Testování s napodobnou architekturou – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: 790e077c5b30c4a68a96b3c1a99b40893b2bbe55
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181563"
---
# <a name="testing-with-a-mocking-framework"></a><span data-ttu-id="749a5-102">Testování pomocí makety architektury</span><span class="sxs-lookup"><span data-stu-id="749a5-102">Testing with a mocking framework</span></span>
> [!NOTE]
> <span data-ttu-id="749a5-103">**EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="749a5-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="749a5-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="749a5-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="749a5-105">Při psaní testů pro aplikaci je často žádoucí vyhnout se tomu, aby se databáze mohla zacházet.</span><span class="sxs-lookup"><span data-stu-id="749a5-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="749a5-106">Entity Framework vám umožňuje dosáhnout toho, že vytvoří kontext – s chováním definovanými testy, které využívá data v paměti.</span><span class="sxs-lookup"><span data-stu-id="749a5-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="749a5-107">Možnosti pro vytváření dvojitých hodnot testu</span><span class="sxs-lookup"><span data-stu-id="749a5-107">Options for creating test doubles</span></span>  

<span data-ttu-id="749a5-108">Existují dva různé přístupy, které lze použít k vytvoření verze vašeho kontextu v paměti.</span><span class="sxs-lookup"><span data-stu-id="749a5-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="749a5-109">**Vytvořte si vlastní test Doubles** – tento přístup zahrnuje zápis vlastní implementace v paměti vašeho kontextu a DbSets.</span><span class="sxs-lookup"><span data-stu-id="749a5-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="749a5-110">Díky tomu máte spoustu kontroly nad tím, jak se třídy chovají, ale mohou zahrnovat psaní a vlastnictví přiměřeného kódu.</span><span class="sxs-lookup"><span data-stu-id="749a5-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="749a5-111">**Použijte napodobnou architekturu k vytvoření dvojitých testů** – pomocí napodobení rozhraní (například MOQ), můžete mít implementace kontextu v paměti a sady se vytvoří dynamicky za běhu za vás.</span><span class="sxs-lookup"><span data-stu-id="749a5-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of your context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="749a5-112">Tento článek se zabývat používáním napodobné architektury.</span><span class="sxs-lookup"><span data-stu-id="749a5-112">This article will deal with using a mocking framework.</span></span> <span data-ttu-id="749a5-113">Pro vytvoření vlastního testu dvakrát se podívejte na [testování s vlastními dvojitými testy](writing-test-doubles.md).</span><span class="sxs-lookup"><span data-stu-id="749a5-113">For creating your own test doubles see [Testing with Your Own Test Doubles](writing-test-doubles.md).</span></span>  

<span data-ttu-id="749a5-114">K demonstraci použití EF s napodobnou architekturou budeme používat MOQ.</span><span class="sxs-lookup"><span data-stu-id="749a5-114">To demonstrate using EF with a mocking framework we are going to use Moq.</span></span> <span data-ttu-id="749a5-115">Nejjednodušší způsob, jak získat MOQ, je instalace [balíčku MOQ z NuGet](https://nuget.org/packages/Moq/).</span><span class="sxs-lookup"><span data-stu-id="749a5-115">The easiest way to get Moq is to install the [Moq package from NuGet](https://nuget.org/packages/Moq/).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="749a5-116">Testování s EF6 verzemi</span><span class="sxs-lookup"><span data-stu-id="749a5-116">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="749a5-117">Scénář uvedený v tomto článku závisí na některých změnách, které jsme provedli v Negenerickými v EF6.</span><span class="sxs-lookup"><span data-stu-id="749a5-117">The scenario shown in this article is dependent on some changes we made to DbSet in EF6.</span></span> <span data-ttu-id="749a5-118">Pro testování pomocí EF5 a starší verze se podívejte na [testování s falešným kontextem](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="749a5-118">For testing with EF5 and earlier version see [Testing with a Fake Context](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="749a5-119">Omezení pro dvojité testy v paměti EF</span><span class="sxs-lookup"><span data-stu-id="749a5-119">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="749a5-120">Dvojitá přesnost testu paměti může být dobrým způsobem, jak poskytnout jednotkové pokrytí na úrovni testování bitů vaší aplikace, které používají EF.</span><span class="sxs-lookup"><span data-stu-id="749a5-120">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="749a5-121">Nicméně když to uděláte, budete používat LINQ to Objects k provádění dotazů na data v paměti.</span><span class="sxs-lookup"><span data-stu-id="749a5-121">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="749a5-122">Výsledkem může být odlišné chování než použití zprostředkovatele LINQ (LINQ to Entities) EF k překladu dotazů na SQL, které se spouští proti vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="749a5-122">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="749a5-123">Jedním z příkladů takového rozdílu je načítání souvisejících dat.</span><span class="sxs-lookup"><span data-stu-id="749a5-123">One example of such a difference is loading related data.</span></span> <span data-ttu-id="749a5-124">Pokud vytvoříte řadu blogů, které mají všechny související příspěvky, pak při použití dat v paměti budou související příspěvky vždy načteny pro každý blog.</span><span class="sxs-lookup"><span data-stu-id="749a5-124">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="749a5-125">Při spuštění na databázi však budou data načtena pouze v případě, že použijete metodu include.</span><span class="sxs-lookup"><span data-stu-id="749a5-125">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="749a5-126">Z tohoto důvodu se doporučuje vždy zahrnout určitou úroveň komplexního testování (kromě testů jednotek), abyste zajistili správné fungování aplikace proti databázi.</span><span class="sxs-lookup"><span data-stu-id="749a5-126">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="749a5-127">Následující článek spolu s tímto článkem</span><span class="sxs-lookup"><span data-stu-id="749a5-127">Following along with this article</span></span>  

<span data-ttu-id="749a5-128">Tento článek obsahuje kompletní výpisy kódu, které můžete zkopírovat do sady Visual Studio, abyste mohli postupovat podle toho, co chcete.</span><span class="sxs-lookup"><span data-stu-id="749a5-128">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="749a5-129">Je nejjednodušší vytvořit **projekt testování částí** a vy budete muset cílit **.NET Framework 4,5** , abyste dokončili oddíly, které používají async.</span><span class="sxs-lookup"><span data-stu-id="749a5-129">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="the-ef-model"></a><span data-ttu-id="749a5-130">Model EF</span><span class="sxs-lookup"><span data-stu-id="749a5-130">The EF model</span></span>  

<span data-ttu-id="749a5-131">Služba, kterou budeme testovat, používá model EF, který se skládá z BloggingContext a třídy blog a post.</span><span class="sxs-lookup"><span data-stu-id="749a5-131">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="749a5-132">Tento kód byl pravděpodobně vygenerován návrhářem EF nebo se jedná o Code First model.</span><span class="sxs-lookup"><span data-stu-id="749a5-132">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a><span data-ttu-id="749a5-133">Vlastnosti Virtual Negenerickými s nástrojem EF Designer</span><span class="sxs-lookup"><span data-stu-id="749a5-133">Virtual DbSet properties with EF Designer</span></span>  

<span data-ttu-id="749a5-134">Všimněte si, že vlastnosti Negenerickými v kontextu jsou označeny jako virtuální.</span><span class="sxs-lookup"><span data-stu-id="749a5-134">Note that the DbSet properties on the context are marked as virtual.</span></span> <span data-ttu-id="749a5-135">To umožní, aby se napodobující rozhraní dědilo z našeho kontextu a přepsalo tyto vlastnosti s napodobnou implementací.</span><span class="sxs-lookup"><span data-stu-id="749a5-135">This will allow the mocking framework to derive from our context and overriding these properties with a mocked implementation.</span></span>  

<span data-ttu-id="749a5-136">Pokud používáte Code First pak můžete třídy upravit přímo.</span><span class="sxs-lookup"><span data-stu-id="749a5-136">If you are using Code First then you can edit your classes directly.</span></span> <span data-ttu-id="749a5-137">Pokud používáte návrháře EF, budete muset upravit šablonu T4, která generuje váš kontext.</span><span class="sxs-lookup"><span data-stu-id="749a5-137">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="749a5-138">Otevřete \<model_name\>. Soubor Context.tt, který je vnořen do souboru EDMX, vyhledejte následující fragment kódu a přidejte do klíčového slova Virtual, jak je znázorněno na obrázku.</span><span class="sxs-lookup"><span data-stu-id="749a5-138">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the virtual keyword as shown.</span></span>  

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

## <a name="service-to-be-tested"></a><span data-ttu-id="749a5-139">Služba, která se má testovat</span><span class="sxs-lookup"><span data-stu-id="749a5-139">Service to be tested</span></span>  

<span data-ttu-id="749a5-140">K předvedení testování s dvojitými pokusy o testování v paměti budeme psát několik testů pro BlogService.</span><span class="sxs-lookup"><span data-stu-id="749a5-140">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="749a5-141">Služba umožňuje vytvářet nové Blogy (AddBlog) a vracet všechny Blogy seřazené podle názvu (GetAllBlogs).</span><span class="sxs-lookup"><span data-stu-id="749a5-141">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="749a5-142">Kromě GetAllBlogs jsme také poskytli metodu, která asynchronně získá všechny Blogy seřazené podle názvu (GetAllBlogsAsync).</span><span class="sxs-lookup"><span data-stu-id="749a5-142">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

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

## <a name="testing-non-query-scenarios"></a><span data-ttu-id="749a5-143">Testování scénářů nesouvisejících s dotazy</span><span class="sxs-lookup"><span data-stu-id="749a5-143">Testing non-query scenarios</span></span>  

<span data-ttu-id="749a5-144">To je všechno, co musíme udělat pro spuštění testování jiných metod než dotazů.</span><span class="sxs-lookup"><span data-stu-id="749a5-144">That’s all we need to do to start testing non-query methods.</span></span> <span data-ttu-id="749a5-145">Následující test používá MOQ k vytvoření kontextu.</span><span class="sxs-lookup"><span data-stu-id="749a5-145">The following test uses Moq to create a context.</span></span> <span data-ttu-id="749a5-146">Potom vytvoří\<Negenerickými blog\> a vodiče, které se vrátí z vlastnosti Blogy daného kontextu.</span><span class="sxs-lookup"><span data-stu-id="749a5-146">It then creates a DbSet\<Blog\> and wires it up to be returned from the context’s Blogs property.</span></span> <span data-ttu-id="749a5-147">V dalším kroku se pomocí tohoto kontextu vytvoří nový BlogService, který se pak použije k vytvoření nového blogu – pomocí metody AddBlog.</span><span class="sxs-lookup"><span data-stu-id="749a5-147">Next, the context is used to create a new BlogService which is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="749a5-148">Nakonec test ověří, že služba přidala nový blog v kontextu, který se nazývá SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="749a5-148">Finally, the test verifies that the service added a new Blog and called SaveChanges on the context.</span></span>  

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

## <a name="testing-query-scenarios"></a><span data-ttu-id="749a5-149">Testování scénářů dotazů</span><span class="sxs-lookup"><span data-stu-id="749a5-149">Testing query scenarios</span></span>  

<span data-ttu-id="749a5-150">Aby bylo možné provádět dotazy proti Negenerickými testu, musíme nastavit implementaci rozhraní IQueryable.</span><span class="sxs-lookup"><span data-stu-id="749a5-150">In order to be able to execute queries against our DbSet test double we need to setup an implementation of IQueryable.</span></span> <span data-ttu-id="749a5-151">Prvním krokem je vytvoření dat v paměti – používáme seznam\<\>blogu.</span><span class="sxs-lookup"><span data-stu-id="749a5-151">The first step is to create some in-memory data – we’re using a List\<Blog\>.</span></span> <span data-ttu-id="749a5-152">V dalším kroku vytvoříme kontext a Negenerickými\<blog\> potom provedete implementaci implementace IQueryable pro Negenerickými – stačí pouze delegovat poskytovateli LINQ to Objects, který funguje se seznamem\<T\>.</span><span class="sxs-lookup"><span data-stu-id="749a5-152">Next, we create a context and DBSet\<Blog\> then wire up the IQueryable implementation for the DbSet – they’re just delegating to the LINQ to Objects provider that works with List\<T\>.</span></span>  

<span data-ttu-id="749a5-153">Pak můžeme vytvořit BlogService na základě našich testů a zajistit, aby se data, která vrátíme z GetAllBlogs, objednala podle názvu.</span><span class="sxs-lookup"><span data-stu-id="749a5-153">We can then create a BlogService based on our test doubles and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

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
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

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

### <a name="testing-with-async-queries"></a><span data-ttu-id="749a5-154">Testování pomocí asynchronních dotazů</span><span class="sxs-lookup"><span data-stu-id="749a5-154">Testing with async queries</span></span>

<span data-ttu-id="749a5-155">Entity Framework 6 představil sadu rozšiřujících metod, které lze použít k asynchronnímu provedení dotazu.</span><span class="sxs-lookup"><span data-stu-id="749a5-155">Entity Framework 6 introduced a set of extension methods that can be used to asynchronously execute a query.</span></span> <span data-ttu-id="749a5-156">Mezi příklady těchto metod patří ToListAsync, FirstAsync, ForEachAsync atd.</span><span class="sxs-lookup"><span data-stu-id="749a5-156">Examples of these methods include ToListAsync, FirstAsync, ForEachAsync, etc.</span></span>  

<span data-ttu-id="749a5-157">Vzhledem k tomu, že Entity Framework dotazy využívají technologii LINQ, jsou metody rozšíření definovány v rozhraních IQueryable a IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="749a5-157">Because Entity Framework queries make use of LINQ, the extension methods are defined on IQueryable and IEnumerable.</span></span> <span data-ttu-id="749a5-158">Protože jsou však určeny pouze pro použití s Entity Framework při pokusu o použití v dotazu LINQ, který není Entity Framework dotazem, se může zobrazit následující chyba:</span><span class="sxs-lookup"><span data-stu-id="749a5-158">However, because they are only designed to be used with Entity Framework you may receive the following error if you try to use them on a LINQ query that isn’t an Entity Framework query:</span></span>

> <span data-ttu-id="749a5-159">Zdroj IQueryable neimplementuje IDbAsyncEnumerable{0}.</span><span class="sxs-lookup"><span data-stu-id="749a5-159">The source IQueryable doesn't implement IDbAsyncEnumerable{0}.</span></span> <span data-ttu-id="749a5-160">Pro Entity Framework asynchronní operace lze použít pouze zdroje, které implementují IDbAsyncEnumerable.</span><span class="sxs-lookup"><span data-stu-id="749a5-160">Only sources that implement IDbAsyncEnumerable can be used for Entity Framework asynchronous operations.</span></span> <span data-ttu-id="749a5-161">Další podrobnosti najdete v tématu [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).</span><span class="sxs-lookup"><span data-stu-id="749a5-161">For more details see [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).</span></span>  

<span data-ttu-id="749a5-162">Zatímco asynchronní metody jsou podporovány pouze při spuštění proti dotazu EF, je vhodné je použít v testu jednotky při spuštění proti Negenerickými testu v paměti.</span><span class="sxs-lookup"><span data-stu-id="749a5-162">Whilst the async methods are only supported when running against an EF query, you may want to use them in your unit test when running against an in-memory test double of a DbSet.</span></span>  

<span data-ttu-id="749a5-163">Aby bylo možné použít asynchronní metody, musíme pro zpracování asynchronního dotazu vytvořit DbAsyncQueryProvider v paměti.</span><span class="sxs-lookup"><span data-stu-id="749a5-163">In order to use the async methods we need to create an in-memory DbAsyncQueryProvider to process the async query.</span></span> <span data-ttu-id="749a5-164">I když by bylo možné nastavit poskytovatele dotazu pomocí MOQ, je mnohem snazší vytvořit test dvojitou implementaci v kódu.</span><span class="sxs-lookup"><span data-stu-id="749a5-164">Whilst it would be possible to setup a query provider using Moq, it is much easier to create a test double implementation in code.</span></span> <span data-ttu-id="749a5-165">Kód pro tuto implementaci je následující:</span><span class="sxs-lookup"><span data-stu-id="749a5-165">The code for this implementation is as follows:</span></span>  

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

<span data-ttu-id="749a5-166">Teď, když máme asynchronního zprostředkovatele dotazů, můžeme napsat testování částí pro naši novou metodu GetAllBlogsAsync.</span><span class="sxs-lookup"><span data-stu-id="749a5-166">Now that we have an async query provider we can write a unit test for our new GetAllBlogsAsync method.</span></span>  

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
