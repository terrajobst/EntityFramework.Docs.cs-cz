---
title: Konfigurace DbContext – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 0350b25d0d0efe05df7cb9e93a3f4ae2d864fd63
ms.sourcegitcommit: 47e0a66a136e743a815d099d2bee5f0da1a068c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/09/2019
ms.locfileid: "59363934"
---
# <a name="configuring-a-dbcontext"></a>Konfigurace DbContext

Tento článek ukazuje základní vzorce pro konfiguraci `DbContext` prostřednictvím `DbContextOptions` pro připojení k databázi pomocí konkrétního zprostředkovatele EF Core a volitelné chování.

## <a name="design-time-dbcontext-configuration"></a>Konfigurace DbContext v době návrhu

EF Core návrhových nástrojů, jako je například [migrace](xref:core/managing-schemas/migrations/index) musí mít možnost zjistit a vytvořit instanci pracovní `DbContext` aby bylo možné shromažďovat údaje o typy entit a jak jsou mapovány na schéma databáze vaší aplikace. Tento proces může být automatické, tak dlouho, dokud nástroj můžete snadno vytvářet `DbContext` tak, že bude nakonfigurován podobně jak by se nakonfigurovat v době běhu.

Při jakékoli vzor, který obsahuje informace související s konfigurací, které `DbContext` můžete pracovat v době běhu, nástroje, které vyžadují použití `DbContext` v době návrhu lze pracovat pouze s omezený počet vzorů. Ty jsou podrobně popsané v další v [kontextu vytváření návrhu](xref:core/miscellaneous/cli/dbcontext-creation) oddílu.

## <a name="configuring-dbcontextoptions"></a>Konfigurace DbContextOptions

`DbContext` musíte mít instanci `DbContextOptions` aby bylo možné provést žádnou práci. `DbContextOptions` Instance přenáší informace o konfiguraci, jako:

- Poskytovatel databáze, které chcete použít, je obvykle vybrána vyvoláním metody `UseSqlServer` nebo `UseSqlite`. Tyto rozšiřující metody vyžadují odpovídající balíček zprostředkovatele, jako například `Microsoft.EntityFrameworkCore.SqlServer` nebo `Microsoft.EntityFrameworkCore.Sqlite`. Metody jsou definovány v `Microsoft.EntityFrameworkCore` oboru názvů.
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
## <a name="avoiding-dbcontext-threading-issues"></a>Jak se vyhnout DbContext potíže s vlákny

Entity Framework Core nepodporuje více paralelních operací běží na stejné `DbContext` instance. Souběžný přístup může způsobit nedefinované chování, data před poškozením a selhání aplikace. Proto je důležité, vždy používali samostatné `DbContext` instance pro operace, které jsou spuštěny paralelně. 

Existují běžných chyb, které můžete inadvernetly příčina souběžný přístup na stejném `DbContext` instance:

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Zapomínání await pro čekání na dokončení asynchronní operace před zahájením žádné jiné operace stejného DbContext

Asynchronní metody umožňují EF Core k zahájení operace, které neblokující tak přístup k databázi. Ale pokud volající není čekání na dokončení jedné z těchto metod a pokračuje v provádění jiných operací `DbContext`, stav `DbContext` může být (a velmi pravděpodobně bude) poškozený. 

Vždy použít funkci await EF Core asynchronní metody okamžitě.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Implicitně sdílení DbContext instancí napříč několika vlákny pomocí vkládání závislostí

[ `AddDbContext` ](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) Rozšiřující metoda registruje `DbContext` typy [s vymezeným oborem životnost](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) ve výchozím nastavení. 

Toto je před problémy souběžný přístup v aplikacích ASP.NET Core, vzhledem k tomu, že existuje pouze jedno vlákno provádění každý požadavek klienta v daném okamžiku, a vzhledem k tomu, že každý požadavek získá oboru vkládání samostatné závislosti (a tedy samostatný `DbContext` instance).

Nicméně, veškerý kód, který se explicitně spustí více vláken v paralell měli zajistit, aby `DbContext` instancí nejsou nikdy accesed současně.

Pomocí vkládání závislostí, jde tohoto dosáhnout tak, že buď zaregistrujete kontextu jako s vymezeným oborem a vytváření oborů (pomocí `IServiceScopeFactory`) pro každé vlákno, nebo když si zaregistrujete `DbContext` jako přechodné (pomocí přetížení `AddDbContext` desetinný `ServiceLifetime` parametr).

## <a name="more-reading"></a>Další čtení

* Čtení [Začínáme v ASP.NET Core](../get-started/aspnetcore/index.md) Další informace o používání EF s ASP.NET Core.
* Čtení [injektáž závislostí](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) Další informace o používání DI.
* Čtení [testování](testing/index.md) Další informace.
