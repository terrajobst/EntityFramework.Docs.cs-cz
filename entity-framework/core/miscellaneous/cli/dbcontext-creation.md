---
title: Vytváření DbContext při návrhu – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f83d4b16227d114a1cac1514667484a908fea4ac
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197572"
---
<a name="design-time-dbcontext-creation"></a>Vytváření DbContext v době návrhu
==============================
Některé příkazy EF Core nástrojů (například příkazy [migrace][1] ) vyžadují, aby se v době návrhu vytvořila `DbContext` odvozená instance, aby se shromáždily podrobnosti o typech entit aplikace a jak se mapují ke schématu databáze. Ve většině případů je žádoucí, aby byl vytvořen `DbContext` tak, aby byl nakonfigurován podobným způsobem, jak by byl [nakonfigurován v době běhu][2].

Existují různé způsoby, jak se nástroje pokoušejí vytvořit `DbContext`:

<a name="from-application-services"></a>Z aplikačních služeb
-------------------------
Pokud váš projekt po spuštění používá [webového hostitele ASP.NET Core][3] nebo [obecného hostitele .NET Core][4], nástroj se pokusí získat objekt DbContext od poskytovatele služeb aplikace.

Nástroje se nejdřív pokusí získat poskytovatele `Program.CreateHostBuilder()`služby vyvoláním, voláním `Build()`a přístupem `Services` k vlastnosti.

``` csharp
public class Program
{
    public static void Main(string[] args)
        => CreateHostBuilder(args).Build().Run();

    // EF Core uses this method at design time to access the DbContext
    public static IHostBuilder CreateHostBuilder(string[] args)
        => Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(
                webBuilder => webBuilder.UseStartup<Startup>());
}

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
        => services.AddDbContext<ApplicationDbContext>();
}

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

> [!NOTE]
> Při vytváření nové aplikace ASP.NET Core je tento zavěšení součástí standardně.

`DbContext` Samotný a všechny závislosti v konstruktoru musí být registrovány jako služby v poskytovateli služeb aplikace. To lze snadno dosáhnout tím [ `DbContext` , že máte konstruktor na objektu, který převezme instanci `DbContextOptions<TContext>` ][5] [ `AddDbContext<TContext>` ][6]jako argument a pomocí metody.

<a name="using-a-constructor-with-no-parameters"></a>Použití konstruktoru bez parametrů
--------------------------------------
Pokud DbContext nelze získat od poskytovatele služby Application Service, nástroje hledají odvozený `DbContext` typ v rámci projektu. Pak se pokusí vytvořit instanci pomocí konstruktoru bez parametrů. To může být výchozí konstruktor, pokud `DbContext` je nakonfigurován [`OnConfiguring`][7] pomocí metody.

<a name="from-a-design-time-factory"></a>Z továrny v době návrhu
--------------------------
Můžete také sdělit nástroje, jak vytvořit DbContext implementací `IDesignTimeDbContextFactory<TContext>` rozhraní: Pokud třída implementující toto rozhraní je nalezena v jednom projektu jako odvozená `DbContext` nebo v projektu po spuštění aplikace, nástroje vycházejí z dalších způsobů vytvoření DbContext a místo toho používají továrnu v době návrhu.

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
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

> [!NOTE]
> `args` Parametr je aktuálně nepoužit. Existuje [problém][8] sledující možnost zadat argumenty pro dobu návrhu z nástrojů.

Továrna v době návrhu může být obzvláště užitečná, pokud je třeba nakonfigurovat DbContext jinak než v době návrhu, a to v případě, že `DbContext` konstruktor přebírá další parametry nejsou registrovány v di, pokud nepoužíváte di, nebo pokud pro některé důvod, proč nemusíte mít `BuildWebHost` metodu ve `Main` třídě ASP.NET Core vaší aplikace.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
