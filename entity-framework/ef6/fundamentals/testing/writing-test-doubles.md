---
title: Ve Visual Basicu s vlastním testu zdvojnásobí - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: 2158dc73585c2720e7293096b0478c73edf522d9
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490906"
---
# <a name="testing-with-your-own-test-doubles"></a>Testování s čísly typu Double vlastní test
> [!NOTE]
> **EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny informace neplatí.  

Při psaní testů pro vaše aplikace je často žádoucí vyhnout se dosažení databáze.  Entity Framework umožňuje dosáhnout vytvořením kontextu – chování definované testování – která používá data v paměti.  

## <a name="options-for-creating-test-doubles"></a>Možnosti pro vytvoření testu zdvojnásobí  

Existují dva různé přístupy, které lze použít k vytvoření v paměti verze kontextu.  

- **Vytvoření vlastního testu zdvojnásobí** – tento postup zahrnuje zápis vlastní implementaci kontextu a DbSets v paměti. To vám přináší značnou kontroly nad jak třídy chovat, ale může zahrnovat zápis a vlastnící přiměřené kódu.  
- **Napodobování rozhraní framework použít k vytvoření testu zdvojnásobí** – využívá napodobování architekturu (například Moq) můžete mít v paměti implementace je kontext a sady pro vás vytvořili dynamicky za běhu.  

V tomto článku se bude zabývat vytváření vlastních testů double. Informace o použití napodobování framework naleznete v tématu [testování pomocí rozhraní napodobování](mocking.md).  

## <a name="testing-with-pre-ef6-versions"></a>Ve Visual Basicu s EF6 předběžné verze  

Je kompatibilní s EF6 kódu uvedeného v tomto článku. Ve Visual Basicu s EF5 a starší verze najdete v části [testování s kontextem falešné](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Omezení hodnot datového typu Double EF test v paměti  

V paměti testu zdvojnásobí může být dobrým způsobem, jak poskytnout bitů vaší aplikace, které používají EF úrovně pokrytí testování částí. Ale při tomto postupu použijete LINQ to Objects k provádění dotazů na data v paměti. Výsledkem může být odlišné chování než pomocí EF pro zprostředkovatele LINQ (LINQ to Entities) ke dotazy SQL, který běží na vaší databázi.  

Příkladem takových rozdíl načítá související data. Pokud vytvoříte sérii blogů, které mají související příspěvky, pak při používání dat v paměti související příspěvky se vždy načteno pro každý Blog. Ale při spuštění pro databázi data pouze se načtou Pokud použijete metodu zahrnout.  

Z tohoto důvodu se doporučuje vždy zahrnovat určitou úroveň začátku do konce testování (navíc k testování částí) pro zajištění vaší aplikace pracuje správně proti databázi.  

## <a name="following-along-with-this-article"></a>Následující spolu se v tomto článku  

Tento článek obsahuje kompletní kód výpisů, které můžete zkopírovat do sady Visual Studio chcete postup sledovat, pokud chcete. Je nejjednodušší vytvořit **projekt testu jednotek** a budete potřebovat k cíli **rozhraní .NET Framework 4.5** dokončete oddíly, které použít operátory async.  

## <a name="creating-a-context-interface"></a>Vytvoření rozhraní pro kontext  

Vytvoříme si prohlédnout službu, která využívá EF testování modelu. Aby bylo možné nahradit náš kontext EF s verzí v paměti pro testování, budeme definovat rozhraní, že náš EF kontext (a v paměti double) bude imeplement.  

Na službu, kterou budeme testování dotazování a úpravy dat pomocí vlastnosti DbSet náš kontext a také volána metoda SaveChanges vložíte změny do databáze. Proto jsme zahrnutí těchto členů rozhraní.  

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

## <a name="the-ef-model"></a>EF modelu  

Chceme otestovat služba využívá EF modelu tvořené BloggingContext a třídy blogu a příspěvku. Tento kód může být vygenerováno EF designeru nebo modelu Code First.  

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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a>Implementace rozhraní kontextu s EF designeru  

Všimněte si, že náš kontext implementuje rozhraní IBloggingContext.  

Pokud používáte Code First můžete upravit kontext přímo k implementaci rozhraní. Pokud používáte EF designeru pak bude nutné upravit šablonu T4, která generuje váš kontext. Otevřete \<název_modelu\>. Context.TT souboru, která je vnořená v rámci které souboru edmx, vyhledejte následující fragment kódu a přidejte rozhraní, jak je znázorněno.  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
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

<a name="creating-the-in-memory-test-doubles"/> ## Vytvoření testu v paměti zdvojnásobuje  

Když teď máme skutečné modelu EF a služby, můžete použít, je čas vytvořit test v paměti double, můžeme použít pro testování. Vytvořili jsme TestContext test double pro náš kontext. V testu zdvojnásobí, kterou dostaneme k zvolte chování chceme, aby bylo možné podporovat testy budeme ke spuštění. V tomto příkladu jsme právě zachycení počet pokusů, které je volána metoda SaveChanges, ale může obsahovat libovolnou logiku, je potřeba ověřit scénář, který testujete.  

Vytvořili jsme také TestDbSet, poskytující DbSet implementace v paměti. Poskytujeme kompletní čerpat pro všechny metody v DbSet (s výjimkou najít), ale potřebujete implementovat členy, které budou používat testovací scénář.  

TestDbSet využívá některé infrastruktury třídy, které jsme zahrnuli zajistit, že asynchronní dotazy můžete zpracovávat.  

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

### <a name="implementing-find"></a>Implementace hledání  

Metoda hledání je obtížné implementovat obecně. Pokud je potřeba testovat kód, který provádí použijte metodu najít, je nejjednodušší vytvořit test najít DbSet pro všechny typy entit, které potřebují podporu. Poté můžete napsat logiku k vyhledání konkrétního typu entity, jak je znázorněno níže.  

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

## <a name="writing-some-tests"></a>Zápis některé testy  

To je všechno, co musíme udělat pro začátek testování. Následující test vytvoří TestContext a pak je služba založená na tomto kontextu. Služba potom slouží k vytvoření nového blogu – AddBlog metodou. Nakonec test ověří, zda služba přidání nového blogu k vlastnosti objektu context blogy a volána metoda SaveChanges pro daný kontext.  

Toto je jenom pro příklad druhy věcí, které můžete otestovat s dvojitou testu v paměti a můžeme upravit logiku testu zdvojnásobí a ověřování podle svých požadavků.  

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

Tady je další příklad testu – tentokrát ten, který do searche zadá dotaz. Test začíná tím, že vytvoříte kontextu testu s daty v jeho Blogovém vlastnost – Všimněte si, že data nejsou v abecedním pořadí. Pak můžeme vytvořit BlogService na základě našich kontextu testu a ujistěte se, že data, která jsme získat zpět z GetAllBlogs je seřazen podle názvu.  

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

A konečně, budeme psát jeden další test, který využívá naše asynchronní metody a zkontrolujte, že jsme součástí infrastruktury asynchronní [TestDbSet](#creating-the-in-memory-test-doubles) funguje.  

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
