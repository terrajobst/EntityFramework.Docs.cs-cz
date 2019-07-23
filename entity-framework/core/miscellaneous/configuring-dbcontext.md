---
title: Konfigurace DbContext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: ddabf825ef23c2ec07efcde390df7d0cf48db33c
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306513"
---
# <a name="configuring-a-dbcontext"></a>Konfigurace DbContext

Tento článek ukazuje základní vzory pro konfiguraci `DbContext` přes a `DbContextOptions` pro připojení k databázi pomocí konkrétního poskytovatele EF Core a volitelného chování.

## <a name="design-time-dbcontext-configuration"></a>Konfigurace DbContext v době návrhu

EF Core nástroje pro návrh, jako třeba [migrace](xref:core/managing-schemas/migrations/index) , musí být schopné zjistit a vytvořit pracovní instanci `DbContext` typu, aby bylo možné shromáždit podrobnosti o typech entit aplikace a jak se mapují ke schématu databáze. Tento proces může být automatický, dokud ho nástroj snadno vytvoří `DbContext` tak, aby byl nakonfigurován podobně jako v době běhu.

I když jakýkoliv model, který poskytuje potřebné informace o konfiguraci `DbContext` pro nástroj může pracovat v době běhu, mohou nástroje, které `DbContext` vyžadují použití při návrhu, pracovat pouze s omezeným počtem vzorů. Tyto informace jsou podrobněji popsány v části [vytváření kontextu při návrhu](xref:core/miscellaneous/cli/dbcontext-creation) .

## <a name="configuring-dbcontextoptions"></a>Konfigurace DbContextOptions

`DbContext`aby bylo možné provést jakoukoli `DbContextOptions` práci, je nutné, aby měla instance. `DbContextOptions` Instance přináší konfigurační informace, jako například:

- Poskytovatel databáze, který má být použit, který je obvykle vybrán vyvoláním `UseSqlServer` metody `UseSqlite`, jako je například nebo. Tyto metody rozšíření vyžadují odpovídající balíček poskytovatele, například `Microsoft.EntityFrameworkCore.SqlServer` nebo. `Microsoft.EntityFrameworkCore.Sqlite` Metody jsou definovány v `Microsoft.EntityFrameworkCore` oboru názvů.
- Jakýkoli potřebný připojovací řetězec nebo identifikátor instance databáze, který se obvykle předává jako argument pro metodu výběru poskytovatele uvedenou výše.
- Jakékoli volitelné selektory chování na úrovni poskytovatele, obvykle také zřetězené uvnitř volání metody výběru poskytovatele
- Všechny obecné selektory chování EF Core, obvykle zřetězené po nebo před metodou selektoru poskytovatele

Následující příklad nakonfiguruje `DbContextOptions` , aby používal poskytovatele SQL Server, připojení obsažené `connectionString` v proměnné, časový limit příkazu na úrovni poskytovatele a selektor chování EF Core, který provádí `DbContext` všechny dotazy spouštěné v [bez sledování](xref:core/querying/tracking#no-tracking-queries) ve výchozím nastavení:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Metody selektoru poskytovatele a další výše uvedené metody selektoru chování `DbContextOptions` jsou rozšiřující metody na třídy možností nebo specifických pro konkrétní poskytovatele. Aby bylo možné mít přístup k těmto metodám rozšíření, možná budete muset mít obor názvů (obvykle `Microsoft.EntityFrameworkCore`) v oboru a zahrnout další závislosti balíčků v projektu.

Lze dodávat do rozhraní `DbContext` přepsáním `OnConfiguring` metody nebo externě prostřednictvím argumentu konstruktoru. `DbContextOptions`

V `OnConfiguring` případě použití obou se použije jako poslední a může přepsat možnosti zadané v argumentu konstruktoru.

### <a name="constructor-argument"></a>Argument konstruktoru

Kód kontextu s konstruktorem:

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
> Základní konstruktor DbContext také přijímá neobecnou verzi `DbContextOptions`, ale použití neobecné verze není doporučeno pro aplikace s více typy kontextu.

Kód aplikace pro inicializaci z argumentu konstruktoru:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>Probíhá konfigurace

Kontextový kód `OnConfiguring`:

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

Kód aplikace pro inicializaci `DbContext` , který používá: `OnConfiguring`

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Tento přístup nezpůsobí testování, pokud testy cílí na úplnou databázi.

### <a name="using-dbcontext-with-dependency-injection"></a>Použití DbContext se vkládáním závislostí

EF Core podporuje použití `DbContext` s kontejnerem vkládání závislostí. Typ DbContext lze přidat do kontejneru služby pomocí `AddDbContext<TContext>` metody.

`AddDbContext<TContext>`provede pro vkládání z kontejneru služby buď `TContext`typ DbContext, a `DbContextOptions<TContext>` odpovídající k dispozici.

Další informace o vkládání závislostí najdete v [Další](#more-reading) části o přečtení.

`DbContext` Přidání do injektáže závislosti:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

To vyžaduje přidání [argumentu konstruktoru](#constructor-argument) do typu DbContext, který přijímá `DbContextOptions<TContext>`.

Kód kontextu:

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

Kód aplikace (s použitím ServiceProvider přímo, méně časté):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a>Předcházení problémům s vlákny DbContext

Entity Framework Core nepodporuje spouštění více paralelních operací ve stejné `DbContext` instanci. To zahrnuje paralelní spuštění asynchronních dotazů a jakékoli explicitní souběžné použití z více vláken. Proto vždy `await` asynchronní hovory okamžitě nebo pro operace, které `DbContext` se spouštějí paralelně, použijte samostatné instance.

Pokud EF Core zjistí, že se pokus o `DbContext` použití instance souběžně používá, `InvalidOperationException` zobrazí se zpráva podobná této: 

> Druhá operace začala v tomto kontextu před dokončením předchozí operace. To je obvykle způsobeno různými vlákny pomocí stejné instance DbContext, ale členy instance nejsou zaručeny jako bezpečné pro přístup z více vláken.

Pokud se souběžný přístup nedetekuje, může to mít za následek nedefinované chování, chyby aplikace a poškození dat.

Existují běžné chyby, které můžou neúmyslně způsobit souběžný přístup ke `DbContext` stejné instanci:

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Forgetting na čekání na dokončení asynchronní operace před spuštěním jakékoli jiné operace na stejném DbContext

Asynchronní metody umožňují EF Core iniciovat operace, které přistupují k databázi neblokujícím způsobem. Pokud však volající neočekává dokončení jedné z těchto metod a pokračuje v provádění jiných operací na `DbContext`, `DbContext` může být stav objektu, (a velmi pravděpodobně bude) poškozen. 

Vždy čekají EF Core asynchronní metody okamžitě.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Implicitní sdílení instancí DbContext napříč více vlákny prostřednictvím injektáže závislosti

Metoda rozšíření registruje `DbContext` typy s [rozsahem životnosti](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) ve výchozím nastavení. [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) 

To je bezpečné před souběžnými problémy s přístupem v aplikacích ASP.NET Core, protože v daný okamžik probíhá pouze jedno vlákno, které spouští jednotlivé žádosti klienta, a protože každý požadavek získá samostatný rozsah vkládání závislostí (a `DbContext` tudíž samostatné instance).

Nicméně jakýkoli kód, který explicitně spustí více vláken paralelně, by `DbContext` měl zajistit, aby se instance nikdy nepoužily současně.

Pomocí injektáže závislostí lze dosáhnout toho, že buď zaregistrujete kontext jako obor a vytvoříte obory (pomocí `IServiceScopeFactory`) pro každé vlákno, nebo `DbContext` registrací jako přechodný `AddDbContext` (pomocí přetížení, které přebírá `ServiceLifetime` parametr).

## <a name="more-reading"></a>Další čtení

* Další informace o použití EF s ASP.NET Core najdete [v Začínáme ASP.NET Core](../get-started/aspnetcore/index.md) .
* Přečtěte si [Injektáže závislostí](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) , kde se dozvíte víc o používání di.
* Další informace najdete v tématu [testování](testing/index.md) .
