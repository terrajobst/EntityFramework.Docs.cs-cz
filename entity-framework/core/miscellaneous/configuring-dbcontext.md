---
title: Konfigurace DbContext – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 393349c05ffaf42c6d2520e73abce23def6becc0
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995935"
---
# <a name="configuring-a-dbcontext"></a>Konfigurace DbContext

Tento článek ukazuje základní vzorce pro konfiguraci `DbContext` prostřednictvím `DbContextOptions` pro připojení k databázi pomocí konkrétního zprostředkovatele EF Core a volitelné chování.

## <a name="design-time-dbcontext-configuration"></a>Konfigurace DbContext v době návrhu

EF Core návrhových nástrojů, jako je například [migrace](xref:core/managing-schemas/migrations/index) musí mít možnost zjistit a vytvořit instanci pracovní `DbContext` aby bylo možné shromažďovat údaje o typy entit a jak jsou mapovány na schéma databáze vaší aplikace. Tento proces může být automatické, tak dlouho, dokud nástroj můžete snadno vytvářet `DbContext` tak, že bude nakonfigurován podobně jak by se nakonfigurovat v době běhu.

Při jakékoli vzor, který obsahuje informace související s konfigurací, které `DbContext` můžete pracovat v době běhu, nástroje, které vyžadují použití `DbContext` v době návrhu lze pracovat pouze s omezený počet vzorů. Ty jsou podrobně popsané v další v [kontextu vytváření návrhu](xref:core/miscellaneous/cli/dbcontext-creation) oddílu.

## <a name="configuring-dbcontextoptions"></a>Konfigurace DbContextOptions

`DbContext` musíte mít instanci `DbContextOptions` aby bylo možné provést žádnou práci. `DbContextOptions` Instance přenáší informace o konfiguraci, jako:

- Poskytovatel databáze, které chcete použít, je obvykle vybrána vyvoláním metody `UseSqlServer` nebo `UseSqlite`
- Všechny nezbytné připojovací řetězec nebo identifikátor instance databáze obvykle předat jako argument výše popsané metody výběru zprostředkovatele
- Libovolné volitelné chování zprostředkovatele úroveň selektory, obvykle také zřetězené uvnitř volání metody výběru zprostředkovatele
- Žádné obecné EF Core chování selektory, obvykle zřetězené po nebo před voláním metody výběru zprostředkovatele

Následující příklad nastaví `DbContextOptions` použití zprostředkovatele SQL Server, součástí připojení `connectionString` proměnnou, časový limit příkazu úrovni zprostředkovatele a výběr chování EF Core, která provádí všechny dotazy spouštěné v `DbContext` [bez sledování](xref:core/querying/tracking#no-tracking-queries) ve výchozím nastavení:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Výběr metody poskytovatele a jiné chování metody selektor uvedených výše jsou metody rozšíření na `DbContextOptions` nebo třídy možností specifickým pro zprostředkovatele. Pokud chcete mít přístup k tyto rozšiřující metody, budete muset máte obor názvů (obvykle `Microsoft.EntityFrameworkCore`) v oboru a zahrnout další balíčky závislostí do projektu.

`DbContextOptions` Mohou být poskytnuty `DbContext` tak, že přepíšete `OnConfiguring` metoda nebo externě přes argument konstruktoru.

Pokud se oba používají `OnConfiguring` je použito jako poslední a později jej můžete přepsat možnosti zadaný pro argument konstruktoru.

### <a name="constructor-argument"></a>Argument konstruktoru

Kontext kódu pomocí konstruktoru:

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
> Konstruktor základní třídy DbContext přijímá také obecné verzi `DbContextOptions`, ale pro aplikace s více typy kontextu není doporučeno používat obecné verze.

Aplikace kód pro inicializaci z argumentu konstruktoru:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

Kontext kódu pomocí `OnConfiguring`:

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

Aplikace kód pro inicializaci `DbContext` , která používá `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Tento přístup se nepropůjčuje s testováním, není-li testy používat úplnou databázi.

### <a name="using-dbcontext-with-dependency-injection"></a>Pomocí vkládání závislostí DbContext

EF Core podporuje používání `DbContext` s kontejner vkládání závislostí. Váš typ DbContext můžete přidat do kontejneru služby s použitím `AddDbContext<TContext>` metody.

`AddDbContext<TContext>` provede oba váš typ DbContext `TContext`a odpovídající `DbContextOptions<TContext>` k dispozici pro vkládání z kontejneru služeb.

Zobrazit [další čtení](#more-reading) níže pro další informace o vkládání závislostí.

Přidávání `Dbcontext` pro vkládání závislostí:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

K tomu je potřeba přidat [argument konstruktoru](#constructor-argument) do typu DbContext, který přijímá `DbContextOptions<TContext>`.

Kontext kódu:

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
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

Kód aplikace (pomocí ServiceProvider přímo, méně běžné):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a>Další čtení

* Čtení [Začínáme v ASP.NET Core](../get-started/aspnetcore/index.md) Další informace o používání EF s ASP.NET Core.
* Čtení [injektáž závislostí](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) Další informace o používání DI.
* Čtení [testování](testing/index.md) Další informace.
