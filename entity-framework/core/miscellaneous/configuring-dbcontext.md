---
title: "Konfigurace DbContext - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 6980acd53b0a74055af7a1e04b476f4625c327c9
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2018
---
# <a name="configuring-a-dbcontext"></a>Konfigurace DbContext

Tento článek ukazuje základní vzory pro konfiguraci `DbContext` prostřednictvím `DbContextOptions` pro připojení k databázi pomocí konkrétního zprostředkovatele EF jádra a volitelné chování.

## <a name="design-time-dbcontext-configuration"></a>Konfigurace DbContext během návrhu

EF základní návrhu nástroje jako [migrace](xref:core/managing-schemas/migrations/index) musí být schopni zjistit a vytvořit funkční instanci `DbContext` typu, aby bylo možné shromáždit informace o typy entit a jak jsou mapovány na schéma databáze aplikace. Tento proces může být automatická, tak dlouho, dokud tento nástroj můžete snadno vytvořit `DbContext` tak, že bude nakonfigurován podobně jak by se nakonfigurovat při spuštění.

Při jakékoli vzor, který obsahuje informace nezbytné konfigurace, které `DbContext` můžete pracovat v době spuštění, nástroje, které vyžadují použití `DbContext` v době návrhu můžete pracovat pouze s omezený počet vzory. Ty jsou popsané v podrobněji [vytvoření kontextu návrhu](xref:core/miscellaneous/cli/dbcontext-creation) části.

## <a name="configuring-dbcontextoptions"></a>Konfigurace DbContextOptions

`DbContext` musíte mít instanci `DbContextOptions` za účelem provedení veškerou práci. `DbContextOptions` Instance představuje informace o konfiguraci, jako například:

- Zprostředkovatel databáze, které chcete použít, je obvykle vybrána vyvoláním metody `UseSqlServer` nebo `UseSqlite`
- Všechny nezbytné připojovací řetězec nebo identifikátor instance databáze obvykle předat jako argument výše popsané metody výběru zprostředkovatele
- Budete potřebovat volitelné chování zprostředkovatele úrovni, obvykle se také zřetězené uvnitř volání metody výběru zprostředkovatele
- Obecné základní EF chování budete potřebovat, obvykle zřetězené po nebo před voláním metody selektor zprostředkovatele

Následující příklad konfiguruje `DbContextOptions` použití zprostředkovatele SQL Server, připojení součástí `connectionString` proměnné, vypršení časového limitu příkaz úrovni zprostředkovatele a selektor chování EF jádra, která umožňuje všechny dotazy spouštěné v `DbContext` [ne sledování](xref:core/querying/tracking#no-tracking-queries) ve výchozím nastavení:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Zprostředkovatel pro výběr metody a jiné chování pro výběr metody uvedené výše jsou rozšiřující metody na `DbContextOptions` nebo třídy možnost specifický pro zprostředkovatele. Chcete-li mít přístup k tyto rozšiřující metody, budete muset obor názvů (obvykle `Microsoft.EntityFrameworkCore`) v oboru a obsahovat závislosti, další balíčky v projektu.

`DbContextOptions` Mohou být poskytnuty `DbContext` přepsáním `OnConfiguring` metoda nebo externě prostřednictvím argument konstruktoru.

Pokud se používá, `OnConfiguring` je použito jako poslední a můžete přepsat možnosti zadaný pro argument konstruktoru.

### <a name="constructor-argument"></a>Argument konstruktoru

Kontext kód s konstruktorem:

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
> Základní konstruktoru DbContext také přijímá neobecnou verzi `DbContextOptions`, ale pro aplikace s více typy kontextu se nedoporučuje používat na neobecnou verzi.

Kód aplikace k chybě při inicializaci z argument konstruktoru:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

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

Kód aplikace k chybě při inicializaci `DbContext` používající `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Tento přístup není jít o testování, pokud testy používat úplnou databázi.

### <a name="using-dbcontext-with-dependency-injection"></a>DbContext pomocí vkládání závislostí

Základní EF podporuje používání `DbContext` s kontejner vkládání závislostí. Váš typ DbContext můžete přidat do kontejneru služby pomocí `AddDbContext<TContext>` metoda.

`AddDbContext<TContext>` budou obě váš typ DbContext, `TContext`a odpovídající `DbContextOptions<TContext>` k dispozici pro vkládání z kontejneru služby.

V tématu [další čtení](#more-reading) níže Další informace o vkládání závislostí.

Přidávání `Dbcontext` k vkládání závislostí:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

To vyžaduje, přidávání [argument konstruktoru](#constructor-argument) pro váš typ DbContext, který přijímá `DbContextOptions<TContext>`.

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

Kód aplikace (pomocí položku ServiceProvider přímo, méně běžné):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a>Další čtení

* Čtení [Začínáme na ASP.NET Core](../get-started/aspnetcore/index.md) Další informace o používání EF s ASP.NET Core.
* Čtení [vkládání závislostí](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) Další informace o používání DI.
* Čtení [testování](testing/index.md) Další informace.
