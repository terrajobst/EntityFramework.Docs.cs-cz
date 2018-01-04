---
title: "Vytváření návrhu DbContext - EF jádra"
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: a899c474cc45437bff7c82ce5bddeb915b15c3b0
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/22/2017
---
<a name="design-time-dbcontext-creation"></a>Vytváření návrhu DbContext
==============================
Některé příkazy EF základní nástroje (například [migrace] [ 1] příkazy) vyžadují odvozeným `DbContext` instance, která má být vytvořen v době návrhu, chcete-li shromažďovat údaje o aplikace typy entit a jak jsou mapovány na schéma databáze. Ve většině případů je žádoucí, který `DbContext` tím vytvořený je nakonfigurovaný podobným způsobem, jak by být [nakonfigurované v době běhu][2].

Existují různé způsoby nástroje pokuste se vytvořit `DbContext`:

<a name="from-application-services"></a>Z aplikačních služeb
-------------------------
Pokud váš projekt po spuštění aplikace ASP.NET Core, nástroje se pokusí získat objekt DbContext z aplikace poskytovatele služeb.

Tento nástroj se nejprve pokusí získat poskytovatele služeb vyvoláním `Program.BuildWebHost()` a přístup `IWebHost.Services` vlastnost.

> [!NOTE]
> Když vytvoříte novou aplikaci ASP.NET 2.0 jádra, je tato háku zahrnuté ve výchozím nastavení. V předchozích verzích EF Core a ASP.NET Core, zkuste vyvolání nástroje `Startup.ConfigureServices` přímo za účelem získání aplikace poskytovatele služeb, ale tento vzor už funguje správně v aplikacích ASP.NET Core 2.0. Pokud provádíte upgrade aplikace ASP.NET Core 1.x na 2.0, můžete [upravit vaše `Program` třída podle vzoru nové][3].

`DbContext` Samostatně a závislostí v jeho konstruktor musí být registrované jako služby ve aplikace poskytovatele služeb. Toho lze snadno dosáhnout tak, že [konstruktor na `DbContext` instanci, která má `DbContextOptions<TContext>` jako argument] [ 4] a pomocí [ `AddDbContext<TContext>` –Metoda] [5].

<a name="using-a-constructor-with-no-parameters"></a>Pomocí konstruktor bez parametrů
--------------------------------------
Pokud DbContext nelze získat ze zprostředkovatele služeb aplikací, nástroje Hledat odvozené `DbContext` typ uvnitř projektu. Potom se pokuste se vytvořit instanci pomocí konstruktor bez parametrů. Může to být výchozí konstruktor. Pokud `DbContext` nakonfigurovaná pomocí [ `OnConfiguring` ] [ 6] metoda.

<a name="from-a-design-time-factory"></a>Z objekt pro vytváření návrhu
--------------------------
Můžete zjistit také nástroje vytvoření vaší DbContext implementací `IDesignTimeDbContextFactory<TContext>` rozhraní: Pokud se nenajde třídu implementace tohoto rozhraní buď stejné projektu jako odvozené `DbContext` nebo v projektu po spuštění aplikace nástroje nepoužívat Další způsoby vytváření DbContext a použití objektu pro vytváření návrhu místo.

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
> `args` Parametr je aktuálně používána. Je [problém] [ 7] možnost zadejte argumenty návrhu z nástroje pro sledování.

Objekt pro vytváření návrhu může být obzvláště užitečný, pokud je nutné nakonfigurovat DbContext odlišně pro dobu návrhu, než v době běhu, pokud `DbContext` konstruktor trvá další parametry nejsou registrované v DI, pokud nepoužíváte DI vůbec, nebo, pokud pro některé důvodu nechcete mít `BuildWebHost` v aplikaci ASP.NET Core – metoda  
`Main`Třída.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
