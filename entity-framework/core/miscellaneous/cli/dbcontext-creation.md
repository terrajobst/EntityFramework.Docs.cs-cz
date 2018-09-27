---
title: Vytváření DbContext v době návrhu – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 66fec7605b6ac2da0af1e801f8a1dca0789aea35
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993715"
---
<a name="design-time-dbcontext-creation"></a>Vytváření DbContext v době návrhu
==============================
Některé příkazy EF Core Tools (například [migrace][1] příkazy) vyžadují odvozené `DbContext` instance má být vytvořen v době návrhu, aby bylo možné získat podrobnosti o vaší aplikace typy entit a jak jsou mapovány na schéma databáze. Ve většině případů je žádoucí, který `DbContext` tím vytvořený nakonfigurovat podobným způsobem, jak by být [nakonfigurované v době běhu][2].

Existují různé způsoby nástroje pokusu o vytvoření `DbContext`:

<a name="from-application-services"></a>Z aplikačních služeb
-------------------------
Pokud je váš projekt po spuštění aplikace ASP.NET Core, nástroje, pokuste se získat objekt DbContext z vaší aplikace poskytovatele služeb.

Nástroje se nejprve pokusí získat poskytovatele služeb vyvoláním `Program.BuildWebHost()` a přístup k `IWebHost.Services` vlastnost.

> [!NOTE]
> Když vytvoříte novou aplikaci ASP.NET Core 2.0, je standardní součástí tohoto připojení. V předchozích verzích EF Core a ASP.NET Core, nástroje zkusit vyvolat `Startup.ConfigureServices` přímo, aby bylo možné získat aplikace poskytovatele služeb, ale tento model už fungování aplikace ASP.NET Core 2.0. Pokud provádíte upgrade aplikace ASP.NET Core 1.x do 2.0, můžete si [upravit vaše `Program` třídy mají nový tvar][3].

`DbContext` Samostatně a všechny závislosti ve svých konstruktorech musí být zaregistrovaní jako služby ve zprostředkovateli služby vaší aplikace. Toho lze snadno dosáhnout tím, že [konstruktoru na `DbContext` , která přijímá instanci `DbContextOptions<TContext>` jako argument][4] a použití [`AddDbContext<TContext>` –Metoda][5].

<a name="using-a-constructor-with-no-parameters"></a>Pomocí konstruktoru bez parametrů
--------------------------------------
Pokud objekt DbContext nelze získat z aplikace poskytovatele služeb, nástrojů Hledat odvozený `DbContext` typu v projektu. Pak se pokusí vytvořit instanci pomocí konstruktoru bez parametrů. To může být výchozí konstruktor, pokud `DbContext` je nakonfigurovaný nástrojem [`OnConfiguring`][6] metody.

<a name="from-a-design-time-factory"></a>Od objektu factory návrhu
--------------------------
Můžete také říct, nástroje pro vytváření vaší DbContext implementací `IDesignTimeDbContextFactory<TContext>` rozhraní: Pokud implementující toto rozhraní se nachází buď stejném projektu jako odvozený `DbContext` nebo v projektu po spuštění aplikace, můžete obejít nástroje Další způsoby vytváření DbContext a použijte příslušný objekt pro vytváření návrhu místo.

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
> `args` Parametr se aktuálně nepoužívá. Je [problém][7] možnost zadat argumenty návrhu z nástrojů pro sledování.

Objekt pro vytváření návrhu může být obzvláště užitečné, pokud je potřeba nakonfigurovat uvolněn objekt DbContext odlišně pro návrhové než v době běhu, pokud `DbContext` konstruktoru přijímá další parametry nejsou registrovány v DI, pokud nepoužíváte DI vůbec nebo, pokud u některých z důvodu nechcete mít `BuildWebHost` metodu v aplikaci ASP.NET Core `Main` třídy.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
