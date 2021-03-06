---
title: Konfigurace DbContext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 53b38f288cd45e66d68ebcc3b6066646d59b0262
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136192"
---
# <a name="configuring-a-dbcontext"></a>Konfigurace DbContext

Tento článek ukazuje základní vzory konfigurace `DbContext` přes `DbContextOptions` pro připojení k databázi pomocí konkrétního poskytovatele EF Core a volitelného chování.

## <a name="design-time-dbcontext-configuration"></a>Konfigurace DbContext v době návrhu

EF Core nástroje pro návrh, jako třeba [migrace](xref:core/managing-schemas/migrations/index) , musí být schopné zjistit a vytvořit pracovní instanci `DbContext` typu, aby bylo možné shromáždit podrobnosti o typech entit aplikace a jak se mapují ke schématu databáze. Tento proces může být automatický, dokud nástroj snadno vytvoří `DbContext` tak, že bude nakonfigurován podobně jako v době běhu.

I když jakýkoliv model, který poskytuje potřebné informace o konfiguraci `DbContext` může pracovat v době běhu, nástroje, které vyžadují použití `DbContext` v době návrhu, můžou fungovat jenom s omezeným počtem vzorů. Tyto informace jsou podrobněji popsány v části [vytváření kontextu při návrhu](xref:core/miscellaneous/cli/dbcontext-creation) .

## <a name="configuring-dbcontextoptions"></a>Konfigurace DbContextOptions

`DbContext` musí mít instanci `DbContextOptions`, aby bylo možné provést jakoukoli práci. Instance `DbContextOptions` obsahuje konfigurační informace, jako například:

- Poskytovatel databáze, který se má použít, který je obvykle vybraný vyvoláním metody, jako je `UseSqlServer` nebo `UseSqlite`. Tyto metody rozšíření vyžadují odpovídající balíček poskytovatele, například `Microsoft.EntityFrameworkCore.SqlServer` nebo `Microsoft.EntityFrameworkCore.Sqlite`. Metody jsou definovány v oboru názvů `Microsoft.EntityFrameworkCore`.
- Jakýkoli potřebný připojovací řetězec nebo identifikátor instance databáze, který se obvykle předává jako argument pro metodu výběru poskytovatele uvedenou výše.
- Jakékoli volitelné selektory chování na úrovni poskytovatele, obvykle také zřetězené uvnitř volání metody výběru poskytovatele
- Všechny obecné selektory chování EF Core, obvykle zřetězené po nebo před metodou selektoru poskytovatele

Následující příklad nakonfiguruje `DbContextOptions` k použití poskytovatele SQL Server, připojení obsaženému v proměnné `connectionString`, časový limit příkazu na úrovni poskytovatele a selektoru EF Core chování, které provádí všechny dotazy spouštěné ve výchozím nastavení `DbContext`ho [No](xref:core/querying/tracking#no-tracking-queries) :

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Metody selektoru poskytovatele a další výše uvedené metody selektoru chování jsou rozšiřující metody pro `DbContextOptions` nebo třídy možností specifické pro poskytovatele. Aby bylo možné mít přístup k těmto metodám rozšíření, může být nutné mít obor názvů (obvykle `Microsoft.EntityFrameworkCore`) v oboru a zahrnovat další závislosti balíčků v projektu.

`DbContextOptions` lze dodávat do `DbContext` přepsáním metody `OnConfiguring` nebo externě prostřednictvím argumentu konstruktoru.

V případě použití obou je `OnConfiguring` použito jako poslední a může přepsat možnosti zadané v argumentu konstruktoru.

### <a name="constructor-argument"></a>Argument konstruktoru

Váš konstruktor může jednoduše přijmout `DbContextOptions` následujícím způsobem:

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

Vaše aplikace teď může předat `DbContextOptions` při vytváření instance kontextu následujícím způsobem:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>Probíhá konfigurace

Můžete také inicializovat `DbContextOptions` v rámci samotného kontextu. I když můžete použít tuto techniku pro základní konfiguraci, obvykle budete potřebovat získat určité podrobnosti o konfiguraci z vnějšku, např. připojovací řetězec databáze. To se dá udělat s rozhraním API konfigurace nebo jakýmkoli jiným způsobem.

Chcete-li inicializovat `DbContextOptions` v rámci kontextu, přepište metodu `OnConfiguring` a zavolejte metody na poskytnuté `DbContextOptionsBuilder`:

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

Aplikace může jednoduše vytvořit instanci takového kontextu, aniž by bylo nutné předávat cokoli do konstruktoru:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Tento přístup nezpůsobí testování, pokud testy cílí na úplnou databázi.

### <a name="using-dbcontext-with-dependency-injection"></a>Použití DbContext se vkládáním závislostí

EF Core podporuje použití `DbContext` s kontejnerem vkládání závislostí. Typ DbContext lze přidat do kontejneru služby pomocí metody `AddDbContext<TContext>`.

`AddDbContext<TContext>` provedou typ DbContext, `TContext`a odpovídající `DbContextOptions<TContext>` k dispozici pro vkládání z kontejneru služby.

Další informace o vkládání závislostí najdete v další části o [přečtení](#more-reading) .

Přidání `DbContext` do injektáže závislosti:

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

Entity Framework Core nepodporuje spouštění více paralelních operací ve stejné instanci `DbContext`. To zahrnuje paralelní spuštění asynchronních dotazů a jakékoli explicitní souběžné použití z více vláken. Proto vždy `await` asynchronní volání okamžitě nebo pro operace, které se spouštějí paralelně, použijte samostatné instance `DbContext`.

Když EF Core detekuje pokus o použití instance `DbContext` souběžně, zobrazí se `InvalidOperationException` zpráva podobná této:

> Druhá operace začala v tomto kontextu před dokončením předchozí operace. To je obvykle způsobeno různými vlákny pomocí stejné instance DbContext, ale členy instance nejsou zaručeny jako bezpečné pro přístup z více vláken.

Pokud se souběžný přístup nedetekuje, může to mít za následek nedefinované chování, chyby aplikace a poškození dat.

Existují běžné chyby, které můžou neúmyslně způsobit souběžný přístup ke stejné instanci `DbContext`:

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Forgetting na čekání na dokončení asynchronní operace před spuštěním jakékoli jiné operace na stejném DbContext

Asynchronní metody umožňují EF Core iniciovat operace, které přistupují k databázi neblokujícím způsobem. Pokud ale volající neočekává dokončení jedné z těchto metod a pokračuje v provádění dalších operací s `DbContext`, může být stav `DbContext` (a velmi pravděpodobně) poškozený.

Vždy čekají EF Core asynchronní metody okamžitě.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Implicitní sdílení instancí DbContext napříč více vlákny prostřednictvím injektáže závislosti

Metoda rozšíření [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) registruje `DbContext` typů s [vymezenou životností](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) ve výchozím nastavení.

To je bezpečné z hlediska souběžného přístupu ve většině aplikací ASP.NET Core, protože každé žádosti klienta provádí pouze jedno vlákno, a protože každý požadavek získá samostatný rozsah vkládání závislostí (a proto samostatnou `DbContext` instanci). Pro model hostování serveru Blazor se používá jeden logický požadavek na udržování uživatelského okruhu Blazor, takže v každém okruhu uživatele je k dispozici jenom jedna instance DbContext s oborem, pokud se používá výchozí rozsah vkládání.

Jakýkoli kód, který explicitně spustí více vláken paralelně, musí zajistit, aby se instance `DbContext` nepoužily současně.

Pomocí injektáže závislostí lze dosáhnout toho, že zaregistrujete kontext jako obor a vytvoříte obory (pomocí `IServiceScopeFactory`) pro každé vlákno nebo registrací `DbContext` jako přechodný (pomocí přetížení `AddDbContext`, které přebírá parametr `ServiceLifetime`).

## <a name="more-reading"></a>Další čtení

- Přečtěte si [Injektáže závislostí](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) , kde se dozvíte víc o používání di.
- Další informace najdete v tématu [testování](testing/index.md) .
