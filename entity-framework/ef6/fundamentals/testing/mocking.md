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
# <a name="testing-with-a-mocking-framework"></a>Testování pomocí makety architektury
> [!NOTE]
> **EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.  

Při psaní testů pro aplikaci je často žádoucí vyhnout se tomu, aby se databáze mohla zacházet.  Entity Framework vám umožňuje dosáhnout toho, že vytvoří kontext – s chováním definovanými testy, které využívá data v paměti.  

## <a name="options-for-creating-test-doubles"></a>Možnosti pro vytváření dvojitých hodnot testu  

Existují dva různé přístupy, které lze použít k vytvoření verze vašeho kontextu v paměti.  

- **Vytvořte si vlastní test Doubles** – tento přístup zahrnuje zápis vlastní implementace v paměti vašeho kontextu a DbSets. Díky tomu máte spoustu kontroly nad tím, jak se třídy chovají, ale mohou zahrnovat psaní a vlastnictví přiměřeného kódu.  
- **Použijte napodobnou architekturu k vytvoření dvojitých testů** – pomocí napodobení rozhraní (například MOQ), můžete mít implementace kontextu v paměti a sady se vytvoří dynamicky za běhu za vás.  

Tento článek se zabývat používáním napodobné architektury. Pro vytvoření vlastního testu dvakrát se podívejte na [testování s vlastními dvojitými testy](writing-test-doubles.md).  

K demonstraci použití EF s napodobnou architekturou budeme používat MOQ. Nejjednodušší způsob, jak získat MOQ, je instalace [balíčku MOQ z NuGet](https://nuget.org/packages/Moq/).  

## <a name="testing-with-pre-ef6-versions"></a>Testování s EF6 verzemi  

Scénář uvedený v tomto článku závisí na některých změnách, které jsme provedli v Negenerickými v EF6. Pro testování pomocí EF5 a starší verze se podívejte na [testování s falešným kontextem](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Omezení pro dvojité testy v paměti EF  

Dvojitá přesnost testu paměti může být dobrým způsobem, jak poskytnout jednotkové pokrytí na úrovni testování bitů vaší aplikace, které používají EF. Nicméně když to uděláte, budete používat LINQ to Objects k provádění dotazů na data v paměti. Výsledkem může být odlišné chování než použití zprostředkovatele LINQ (LINQ to Entities) EF k překladu dotazů na SQL, které se spouští proti vaší databázi.  

Jedním z příkladů takového rozdílu je načítání souvisejících dat. Pokud vytvoříte řadu blogů, které mají všechny související příspěvky, pak při použití dat v paměti budou související příspěvky vždy načteny pro každý blog. Při spuštění na databázi však budou data načtena pouze v případě, že použijete metodu include.  

Z tohoto důvodu se doporučuje vždy zahrnout určitou úroveň komplexního testování (kromě testů jednotek), abyste zajistili správné fungování aplikace proti databázi.  

## <a name="following-along-with-this-article"></a>Následující článek spolu s tímto článkem  

Tento článek obsahuje kompletní výpisy kódu, které můžete zkopírovat do sady Visual Studio, abyste mohli postupovat podle toho, co chcete. Je nejjednodušší vytvořit **projekt testování částí** a vy budete muset cílit **.NET Framework 4,5** , abyste dokončili oddíly, které používají async.  

## <a name="the-ef-model"></a>Model EF  

Služba, kterou budeme testovat, používá model EF, který se skládá z BloggingContext a třídy blog a post. Tento kód byl pravděpodobně vygenerován návrhářem EF nebo se jedná o Code First model.  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a>Vlastnosti Virtual Negenerickými s nástrojem EF Designer  

Všimněte si, že vlastnosti Negenerickými v kontextu jsou označeny jako virtuální. To umožní, aby se napodobující rozhraní dědilo z našeho kontextu a přepsalo tyto vlastnosti s napodobnou implementací.  

Pokud používáte Code First pak můžete třídy upravit přímo. Pokud používáte návrháře EF, budete muset upravit šablonu T4, která generuje váš kontext. Otevřete \<model_name @ no__t-1. Soubor Context.tt, který je vnořen do souboru EDMX, vyhledejte následující fragment kódu a přidejte do klíčového slova Virtual, jak je znázorněno na obrázku.  

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

## <a name="service-to-be-tested"></a>Služba, která se má testovat  

K předvedení testování s dvojitými pokusy o testování v paměti budeme psát několik testů pro BlogService. Služba umožňuje vytvářet nové Blogy (AddBlog) a vracet všechny Blogy seřazené podle názvu (GetAllBlogs). Kromě GetAllBlogs jsme také poskytli metodu, která asynchronně získá všechny Blogy seřazené podle názvu (GetAllBlogsAsync).  

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

## <a name="testing-non-query-scenarios"></a>Testování scénářů nesouvisejících s dotazy  

To je všechno, co musíme udělat pro spuštění testování jiných metod než dotazů. Následující test používá MOQ k vytvoření kontextu. Potom vytvoří Negenerickými @ no__t-0Blog @ no__t-1 a vodiče, aby se vrátily z vlastnosti Blogy daného kontextu. V dalším kroku se pomocí tohoto kontextu vytvoří nový BlogService, který se pak použije k vytvoření nového blogu – pomocí metody AddBlog. Nakonec test ověří, že služba přidala nový blog v kontextu, který se nazývá SaveChanges.  

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

## <a name="testing-query-scenarios"></a>Testování scénářů dotazů  

Aby bylo možné provádět dotazy proti Negenerickými testu, musíme nastavit implementaci rozhraní IQueryable. Prvním krokem je vytvoření některých dat v paměti – používáme seznam @ no__t-0Blog @ no__t-1. V dalším kroku vytvoříme kontext a Negenerickými @ no__t-0Blog @ no__t-1 a potom rozvedete implementaci IQueryable pro Negenerickými – tím pouze delegujete poskytovatel LINQ to Objects, který funguje se seznamem @ no__t-2T @ no__t-3.  

Pak můžeme vytvořit BlogService na základě našich testů a zajistit, aby se data, která vrátíme z GetAllBlogs, objednala podle názvu.  

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

### <a name="testing-with-async-queries"></a>Testování pomocí asynchronních dotazů

Entity Framework 6 představil sadu rozšiřujících metod, které lze použít k asynchronnímu provedení dotazu. Mezi příklady těchto metod patří ToListAsync, FirstAsync, ForEachAsync atd.  

Vzhledem k tomu, že Entity Framework dotazy využívají technologii LINQ, jsou metody rozšíření definovány v rozhraních IQueryable a IEnumerable. Protože jsou však určeny pouze pro použití s Entity Framework při pokusu o použití v dotazu LINQ, který není Entity Framework dotazem, se může zobrazit následující chyba:

> Zdroj IQueryable neimplementuje IDbAsyncEnumerable @ no__t-0. Pro Entity Framework asynchronní operace lze použít pouze zdroje, které implementují IDbAsyncEnumerable. Další podrobnosti najdete [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).  

Zatímco asynchronní metody jsou podporovány pouze při spuštění proti dotazu EF, je vhodné je použít v testu jednotky při spuštění proti Negenerickými testu v paměti.  

Aby bylo možné použít asynchronní metody, musíme pro zpracování asynchronního dotazu vytvořit DbAsyncQueryProvider v paměti. I když by bylo možné nastavit poskytovatele dotazu pomocí MOQ, je mnohem snazší vytvořit test dvojitou implementaci v kódu. Kód pro tuto implementaci je následující:  

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

Teď, když máme asynchronního zprostředkovatele dotazů, můžeme napsat testování částí pro naši novou metodu GetAllBlogsAsync.  

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
