---
title: "Konfigurace DbContext - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 96abf3b94be3e1d19f833644f1c2f6f13fe0e730
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/05/2017
---
# <a name="configuring-a-dbcontext"></a>Konfigurace DbContext

Tento článek popisuje vzory pro konfiguraci `DbContext` s `DbContextOptions`. Možnosti se primárně používají konfigurace úložiště dat a vyberte.

## <a name="configuring-dbcontextoptions"></a>Konfigurace DbContextOptions

`DbContext`musíte mít instanci `DbContextOptions` provádění. To se dá nakonfigurovat přepsání `OnConfiguring`, nebo zadaná externě prostřednictvím argument konstruktoru.

Pokud se používá, `OnConfiguring` se spouští na zadaný možnosti, což znamená, je doplňkové a můžete je zadaný pro argument konstruktoru možnosti přepsání.

### <a name="constructor-argument"></a>Argument konstruktoru

Kontext kódu pomocí konstruktoru

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> Základní konstruktoru DbContext také přijímá neobecnou verzi `DbContextOptions`. Pro aplikace s více typy kontextu se nedoporučuje používat na neobecnou verzi.

Kód aplikace k chybě při inicializaci z argument konstruktoru

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

> [!WARNING]  
> `OnConfiguring`Probíhá poslední a můžete přepsat možnosti získaný od DI nebo konstruktoru. Tento přístup není jít o testování (Pokud cílíte databázi úplná).

Kontext kódu s použitím `OnConfiguring`:

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

Kód aplikace se nezdařila s `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a>DbContext pomocí vkládání závislostí

EF podporuje používání `DbContext` s kontejner vkládání závislostí. Váš typ DbContext můžete přidat do kontejneru služby pomocí `AddDbContext<TContext>`.

`AddDbContext`budou obě váš typ DbContext, `TContext`, a `DbContextOptions<TContext>` k dispozici pro vkládání z kontejneru služby.

V tématu [další čtení](#more-reading) níže informace o vkládání závislostí.

Přidání dbcontext k vkládání závislostí

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

To vyžaduje, přidávání [argument konstruktoru](#constructor-argument) pro váš typ DbContext, který přijímá `DbContextOptions`.

Kontext kód:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

Kód aplikace (v ASP.NET Core):

``` csharp
public MyController(BloggingContext context)
```

Kód aplikace (pomocí položku ServiceProvider přímo, méně běžné):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a>Pomocí`IDesignTimeDbContextFactory<TContext>`

Jako alternativu k výše uvedených možností, může taky poskytnout implementaci `IDesignTimeDbContextFactory<TContext>`. EF nástroje můžete použít k vytvoření instance vaší DbContext tento objekt pro vytváření. To může být potřeba Chcete-li povolit konkrétní návrhu činnosti, jako je například migrace.

Implementace tohoto rozhraní povolit návrhu služby pro kontextové typy, které nemají výchozí veřejný konstruktor. Implementace tohoto rozhraní, které jsou ve stejném sestavení jako kontext odvozené automaticky zjistí použití návrhu služby.

Příklad:

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

## <a name="more-reading"></a>Další čtení

* Čtení [Začínáme na ASP.NET Core](../get-started/aspnetcore/index.md) Další informace o používání EF s ASP.NET Core.
* Čtení [vkládání závislostí](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) Další informace o používání DI.
* Čtení [testování](testing/index.md) Další informace.
