---
title: "Vytváření návrhu DbContext - EF jádra"
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a>Vytváření návrhu DbContext
==============================
Některé nástroje EF příkazy vyžadují instance DbContext má být vytvořen v návrhu čas (například při spuštění migrace příkazů). Existují různé způsoby nástroje pokuste se jej vytvořit.

<a name="from-application-services"></a>Z aplikačních služeb
-------------------------
Pokud váš projekt po spuštění aplikace ASP.NET Core, nástroje se pokusí získat objekt DbContext z aplikace poskytovatele služeb. Získají vyvoláním `Program.BuildWebHost()` a přístup `IWebHost.Services` vlastnost. Všechny DbContext registrovat pomocí `IServiceCollection.AddDbContext<TContext>()` můžete najít a vytvořené tímto způsobem. Tento vzor byl [byla zavedená v technologii ASP.NET 2.0 jádra][1]

<a name="using-the-default-constructor"></a>Pomocí výchozí konstruktor.
-----------------------------
Pokud se DbContext nelze získat ze zprostředkovatele služeb aplikací, podívejte se nástroje pro typ DbContext v projektu. Se pokusí vytvořit pomocí jeho výchozí konstruktor.

<a name="from-a-design-time-factory"></a>Z objekt pro vytváření návrhu
--------------------------
Můžete zjistit také nástroje vytvoření vaší DbContext implementací `IDesignTimeDbContextFactory`. Pokud se najde třídu implementace tohoto rozhraní v projektu, nástroje nepoužívat další způsoby vytváření DbContext.
Vždy používají objekt factory v době návrhu. Objekt je obzvláště užitečné, pokud je nutné nakonfigurovat DbContext odlišně pro dobu návrhu, než v době běhu.

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

> [!NOTE]
> `args` Parametr je aktuálně používána. Je [problém] [ 2] možnost zadejte argumenty návrhu z nástroje pro sledování.

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
