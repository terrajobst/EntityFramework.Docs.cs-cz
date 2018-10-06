---
title: Ve Visual Basicu s napodobování framework – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: 80fd97073744be40d66c09706d3513dba18e724d
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834718"
---
# <a name="testing-with-a-mocking-framework"></a>Testování s napodobování framework
> [!NOTE]
> **EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny informace neplatí.  

Při psaní testů pro vaše aplikace je často žádoucí vyhnout se dosažení databáze.  Entity Framework umožňuje dosáhnout vytvořením kontextu – chování definované testování – která používá data v paměti.  

## <a name="options-for-creating-test-doubles"></a>Možnosti pro vytvoření testu zdvojnásobí  

Existují dva různé přístupy, které lze použít k vytvoření v paměti verze kontextu.  

- **Vytvoření vlastního testu zdvojnásobí** – tento postup zahrnuje zápis vlastní implementaci kontextu a DbSets v paměti. To vám přináší značnou kontroly nad jak třídy chovat, ale může zahrnovat zápis a vlastnící přiměřené kódu.  
- **Napodobování rozhraní framework použít k vytvoření testu zdvojnásobí** – využívá napodobování architekturu (například Moq) můžete mít v paměti implementace je kontext a sady pro vás vytvořili dynamicky za běhu.  

V tomto článku se postará o s využitím napodobování architektury. Vytvoření vlastního testu zdvojnásobí naleznete v tématu [testování se váš vlastní testování hodnot datového typu Double](writing-test-doubles.md).  

Demonstruje použití EF s napodobování framework budeme používat Moq. Nejjednodušší způsob, jak získat Moq je instalace [Moq balíčku od Nugetu](http://nuget.org/packages/Moq/).  

## <a name="testing-with-pre-ef6-versions"></a>Ve Visual Basicu s EF6 předběžné verze  

Scénáře uvedené v tomto článku je závislá na několik změn, které jsme provedli DbSet v EF6. Ve Visual Basicu s EF5 a starší verze najdete v části [testování s kontextem falešné](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Omezení hodnot datového typu Double EF test v paměti  

V paměti testu zdvojnásobí může být dobrým způsobem, jak poskytnout bitů vaší aplikace, které používají EF úrovně pokrytí testování částí. Ale při tomto postupu použijete LINQ to Objects k provádění dotazů na data v paměti. Výsledkem může být odlišné chování než pomocí EF pro zprostředkovatele LINQ (LINQ to Entities) ke dotazy SQL, který běží na vaší databázi.  

Příkladem takových rozdíl načítá související data. Pokud vytvoříte sérii blogů, které mají související příspěvky, pak při používání dat v paměti související příspěvky se vždy načteno pro každý Blog. Ale při spuštění pro databázi data pouze se načtou Pokud použijete metodu zahrnout.  

Z tohoto důvodu se doporučuje vždy zahrnovat určitou úroveň začátku do konce testování (navíc k testování částí) pro zajištění vaší aplikace pracuje správně proti databázi.  

## <a name="following-along-with-this-article"></a>Následující spolu se v tomto článku  

Tento článek obsahuje kompletní kód výpisů, které můžete zkopírovat do sady Visual Studio chcete postup sledovat, pokud chcete. Je nejjednodušší vytvořit **projekt testu jednotek** a budete potřebovat k cíli **rozhraní .NET Framework 4.5** dokončete oddíly, které použít operátory async.  

## <a name="the-ef-model"></a>EF modelu  

Chceme otestovat služba využívá EF modelu tvořené BloggingContext a třídy blogu a příspěvku. Tento kód může být vygenerováno EF designeru nebo modelu Code First.  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a>Vlastnosti virtuální DbSet s EF designeru  

Všimněte si, že vlastnosti DbSet na kontextu, jsou označeny jako virtuální. To vám umožní napodobování framework odvodit z našich kontextu a přepsání těchto vlastností s imitaci implementací.  

Pokud používáte Code First, pak tříd můžete upravovat přímo. Pokud používáte EF designeru pak bude nutné upravit šablonu T4, která generuje váš kontext. Otevřete \<název_modelu\>. Context.TT souboru, která je vnořená v rámci které souboru edmx, najít následující fragment kódu a přidejte do virtual – klíčové slovo, jak je znázorněno.  

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

## <a name="service-to-be-tested"></a>Služba má být testována  

K předvedení testování s čísly typu Double test v paměti budeme psát několik testů pro BlogService. Služba je schopná vytvořit nové blogy (AddBlog) a vrací všechny blogy seřazené podle názvu (GetAllBlogs). Kromě GetAllBlogs poskytujeme také metodu, která asynchronně získá všechny blogy seřazené podle názvu (GetAllBlogsAsync).  

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

## <a name="testing-non-query-scenarios"></a>Scénáře testování bez dotazů  

To je všechno, co musíme udělat pro začátek testování metody bez dotazů. Následující testovací Moq používá k vytvoření kontextu. Pak vytvoří DbSet\<blogu\> a sváže má být vrácena z vlastnosti blogy kontextu. V dalším kroku kontext slouží k vytvoření nové BlogService, které se pak použije k vytvoření nového blogu – AddBlog metodou. Nakonec test ověří, zda služba přidání nového blogu a volána metoda SaveChanges pro daný kontext.  

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

## <a name="testing-query-scenarios"></a>Scénáře testování dotazů  

Aby bylo možné provádět dotazy proti testovacím DbSet double musíme nastavit implementace IQueryable. Prvním krokem je vytvoření některá data v paměti – používáme seznam\<blogu\>. V dalším kroku vytvoříme kontextu a DBSet\<blogu\> potom nastavit IQueryable implementace DbSet – právě jste delegováno na poskytovateli LINQ to Objects, která funguje s seznamu\<T\>.  

Pak můžeme vytvořit BlogService podle našich testu zdvojnásobí a ujistěte se, že data, která jsme získat zpět z GetAllBlogs je seřazen podle názvu.  

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

### <a name="testing-with-async-queries"></a>Testování s asynchronní dotazy

Entity Framework 6 zavedl sadu rozšiřujících metod, které slouží k asynchronně spustí dotaz. Příklady těchto metod: ToListAsync FirstAsync, ForEachAsync, atd.  

Protože Entity Framework dotazy využívají LINQ, rozšiřující metody jsou definovány na IQueryable a IEnumerable. Ale protože jsou určeny pouze pro použití s Entity Framework můžete obdržet následující chybu při pokusu o jejich použití v dotazu LINQ, který není dotaz rozhraní Entity Framework:

> Zdroj IQueryable neimplementuje IDbAsyncEnumerable{0}. Pouze zdroje, které implementují IDbAsyncEnumerable lze použít pro asynchronní operace Entity Framework. Další podrobnosti najdete v tématu [ http://go.microsoft.com/fwlink/?LinkId=287068 ](https://go.microsoft.com/fwlink/?LinkId=287068).  

Zatímco asynchronní metody se podporují jenom při spouštění dotazu EF, můžete chtít použít při spuštění proti v paměti testování double DbSet v testu jednotek.  

Pokud chcete používat asynchronní metody potřebujeme vytvořit DbAsyncQueryProvider v paměti pro zpracování asynchronního dotazu. Zatímco by bylo možné nastavit poskytovatele dotazů pomocí Moq, je snazší vytvářet double provádění testů v kódu. Kód pro tuto implementaci vypadá takto:  

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

Když teď máme poskytovatele asynchronního dotazu jsme pro naši novou metodu GetAllBlogsAsync napsat Jednotkový test.  

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
