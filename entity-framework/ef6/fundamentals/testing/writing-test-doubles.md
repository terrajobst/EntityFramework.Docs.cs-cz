---
title: Testování s vlastním testem dvakrát – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: 3d8933fb5e17f8c01f3971495a1fcdb5b8cfab57
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416327"
---
# <a name="testing-with-your-own-test-doubles"></a>Testování s vlastními dvojitými testy
> [!NOTE]
> **EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.  

Při psaní testů pro aplikaci je často žádoucí vyhnout se tomu, aby se databáze mohla zacházet.  Entity Framework vám umožňuje dosáhnout toho, že vytvoří kontext – s chováním definovanými testy, které využívá data v paměti.  

## <a name="options-for-creating-test-doubles"></a>Možnosti pro vytváření dvojitých hodnot testu  

Existují dva různé přístupy, které lze použít k vytvoření verze vašeho kontextu v paměti.  

- **Vytvořte si vlastní test Doubles** – tento přístup zahrnuje zápis vlastní implementace v paměti vašeho kontextu a DbSets. Díky tomu máte spoustu kontroly nad tím, jak se třídy chovají, ale mohou zahrnovat psaní a vlastnictví přiměřeného kódu.  
- **Použijte napodobnou architekturu k vytvoření dvojitých testů** – pomocí napodobení rozhraní (například MOQ), můžete mít implementace kontextu v paměti a sady se vytvoří dynamicky za běhu za vás.  

Tento článek se zabývat vytvářením vlastního testu dvakrát. Informace o použití rozhraní pro návrhy viz testování s napodobnou [architekturou](mocking.md).  

## <a name="testing-with-pre-ef6-versions"></a>Testování s EF6 verzemi  

Kód uvedený v tomto článku je kompatibilní s EF6. Pro testování pomocí EF5 a starší verze se podívejte na [testování s falešným kontextem](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Omezení pro dvojité testy v paměti EF  

Dvojitá přesnost testu paměti může být dobrým způsobem, jak poskytnout jednotkové pokrytí na úrovni testování bitů vaší aplikace, které používají EF. Nicméně když to uděláte, budete používat LINQ to Objects k provádění dotazů na data v paměti. Výsledkem může být odlišné chování než použití zprostředkovatele LINQ (LINQ to Entities) EF k překladu dotazů na SQL, které se spouští proti vaší databázi.  

Jedním z příkladů takového rozdílu je načítání souvisejících dat. Pokud vytvoříte řadu blogů, které mají všechny související příspěvky, pak při použití dat v paměti budou související příspěvky vždy načteny pro každý blog. Při spuštění na databázi však budou data načtena pouze v případě, že použijete metodu include.  

Z tohoto důvodu se doporučuje vždy zahrnout určitou úroveň komplexního testování (kromě testů jednotek), abyste zajistili správné fungování aplikace proti databázi.  

## <a name="following-along-with-this-article"></a>Následující článek spolu s tímto článkem  

Tento článek obsahuje kompletní výpisy kódu, které můžete zkopírovat do sady Visual Studio, abyste mohli postupovat podle toho, co chcete. Je nejjednodušší vytvořit **projekt testování částí** a vy budete muset cílit **.NET Framework 4,5** , abyste dokončili oddíly, které používají async.  

## <a name="creating-a-context-interface"></a>Vytvoření kontextu rozhraní  

Budeme se pohlížet na testování služby, která využívá model EF. Aby bylo možné nahradit náš kontext EF pomocí verze v paměti pro testování, definujeme rozhraní, které náš kontext EF (a Dvojitá paměť v paměti) provede implementaci.

Služba, kterou budeme testovat, bude dotazovat se na data a upravovat je pomocí vlastností Negenerickými našeho kontextu a volat metodu SaveChanges a doručovat změny do databáze. Proto do rozhraní patříme tyto členy.  

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

## <a name="the-ef-model"></a>Model EF  

Služba, kterou budeme testovat, používá model EF, který se skládá z BloggingContext a třídy blog a post. Tento kód byl pravděpodobně vygenerován návrhářem EF nebo se jedná o Code First model.  

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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a>Implementace kontextu rozhraní s návrhářem EF  

Všimněte si, že náš kontext implementuje rozhraní IBloggingContext.  

Pokud používáte Code First pak můžete upravit kontext přímo k implementaci rozhraní. Pokud používáte návrháře EF, budete muset upravit šablonu T4, která generuje váš kontext. Otevřete \<model_name\>. Soubor Context.tt, který je vnořen do souboru EDMX, vyhledejte následující fragment kódu a přidejte ho do rozhraní, jak je znázorněno na obrázku.  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
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

## <a name="creating-the-in-memory-test-doubles"></a>Vytváření dvojitých hodnot testu v paměti  

Teď, když máme skutečný model EF a službu, která ho může používat, je čas vytvořit zdvojnásobení testu v paměti, které můžeme použít pro testování. Pro náš kontext jsme vytvořili TestContext test Double. V testu zdvojnásobme si, jak zvolit chování, které chceme, aby bylo možné podporovat testy, které budeme spouštět. V tomto příkladu právě zachycujete počet volání metody SaveChanges, ale můžete zahrnout jakoukoli logiku potřebnou k ověření scénáře, který testujete.  

Vytvořili jsme také TestDbSet, který poskytuje implementaci Negenerickými v paměti. K dispozici je kompletní implementace všech metod na Negenerickými (s výjimkou hledání), ale stačí pouze implementovat členy, které bude váš testovací scénář používat.  

TestDbSet využívá některé jiné třídy infrastruktury, které jsme zahrnuli k zajištění toho, aby bylo možné zpracovávat asynchronní dotazy.  

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

### <a name="implementing-find"></a>Implementace Find  

Metodu Find je obtížné implementovat obecným způsobem. Pokud potřebujete testovat kód, který využívá metodu Find, je nejjednodušší vytvořit Negenerickými testu pro každý typ entity, který musí podporovat hledání. Pak můžete napsat logiku k vyhledání konkrétního typu entity, jak je znázorněno níže.  

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

## <a name="writing-some-tests"></a>Zápis některých testů  

To je všechno, co musíme udělat k zahájení testování. Následující test vytvoří TestContext a pak službu na základě tohoto kontextu. Služba se pak použije k vytvoření nového blogu – pomocí metody AddBlog. Nakonec test ověří, že služba přidala nový blog do vlastnosti Blogy daného kontextu a v kontextu byla volána metoda SaveChanges.  

Toto je pouze příklad typů položek, které můžete testovat pomocí zdvojnásobení testu v paměti, a můžete upravit logiku podvojení testu a ověření, aby splňovalo vaše požadavky.  

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

Tady je další příklad testu – tentokrát, který provádí dotaz. Test začíná vytvořením kontextu testu s některými daty ve vlastnosti blogu – Všimněte si, že data nejsou v abecedním pořadí. Na základě našeho kontextu testování můžeme pak vytvořit BlogService a zajistit, aby se data, která vracíme z GetAllBlogs, objednala podle názvu.  

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

Nakonec zapíšeme ještě jeden test, který používá naši asynchronní metodu k zajištění toho, že asynchronní infrastruktura, kterou jsme zahrnuli do [TestDbSet](#creating-the-in-memory-test-doubles) , funguje.  

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
