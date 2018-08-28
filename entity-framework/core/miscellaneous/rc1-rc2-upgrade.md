---
title: Upgrade z verze EF Core 1.0 RC1 na RC2 – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 83b98fda5ac9491994b5b3fb333c9951ec01188a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996894"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>Upgrade z verze EF Core 1.0 RC1 na 1.0 RC2

Tento článek obsahuje pokyny pro přesouvání aplikace sestavené s balíčky RC1 na RC2.

## <a name="package-names-and-versions"></a>Názvy balíčků a verze

Mezi RC1 a RC2 jsme změní z "Entity Frameworku 7" na "Entity Framework Core". Další informace o důvodech, proč pro změnu v hodnotě [tento příspěvek Scotta Hanselmana,](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Z důvodu této změny změnit náš názvy balíčků z `EntityFramework.*` k `Microsoft.EntityFrameworkCore.*` a naše verze `7.0.0-rc1-final` k `1.0.0-rc2-final` (nebo `1.0.0-preview1-final` nástrojů pro).

**Budete muset úplně odebrat RC1 balíčky a pak nainstalujte RC2 těch, které jsou.** Tady je mapování pro některé běžné balíčky.

| Balíček RC1                                               | Ekvivalentní verzi RC2                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final      |
| EntityFramework.SQLite 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final      |
| EntityFramework7.Npgsql                   3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres             <to be advised>      |
| EntityFramework.SqlServerCompact35        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final      |
| EntityFramework.InMemory 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final      |
| EntityFramework.IBMDataServer             7.0.0-beta1     | Zatím není k dispozici pro RC2                                            |
| EntityFramework.Commands 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final      |

## <a name="namespaces"></a>Jmenné prostory

Spolu s názvy balíčků, obory názvů změnil z `Microsoft.Data.Entity.*` k `Microsoft.EntityFrameworkCore.*`. Tato změna se najít/nahradit aplikace dokáže zpracovat `using Microsoft.Data.Entity` s `using Microsoft.EntityFrameworkCore`.

## <a name="table-naming-convention-changes"></a>Tabulky změn konvence názvů

Významné funkční změny jsme provedli v RC2 byl určený název `DbSet<TEntity>` vlastností pro danou entitu jako název tabulky je namapován na, ne jenom název třídy. Další informace o této změně v [problém související oznámení](https://github.com/aspnet/Announcements/issues/167).

Pro existující aplikace RC1, doporučujeme přidat následující kód na začátek vašeho `OnModelCreating` metoda zachovat RC1 taková strategie:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Pokud chcete přijmout nové strategie vytváření názvů, by doporučujeme úspěšně vyplnění zbývající kroky upgradu a pak odstranění kódu a vytváření migrace použít v tabulce přejmenuje.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext / Startup.cs změní (platí pouze pro projekty ASP.NET Core)

V RC1, museli jste přidání Entity Framework služeb k aplikaci poskytovatele služeb – v `Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

Ve verzi RC2, můžete odebrat volání `AddEntityFramework()`, `AddSqlServer()`atd.:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

Také je potřeba přidat konstruktoru, do vaší odvozené kontext, který přebírá kontext možnosti a předává je do konstruktor základní třídy. To je potřeba, proto jsme odstranili některý scary magic, který je v snuck na pozadí:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>Předávání IServiceProvider

Máte RC1 kód, který se předává `IServiceProvider` ke kontextu, to se teď přesunul na `DbContextOptions`, namísto parametru samostatné konstruktoru. Použití `DbContextOptionsBuilder.UseInternalServiceProvider(...)` nastavení poskytovatele služeb.

### <a name="testing"></a>Testování

Nejběžnější scénář to bylo řízení rozsahu databázi InMemory při testování. Zobrazit aktualizovaný [testování](testing/index.md) článku příklad to RC2.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Řešení interních služeb z aplikace poskytovatele služeb (platí pouze pro projekty ASP.NET Core)

Pokud máte aplikace ASP.NET Core a EF vyřešit interních služeb z aplikace poskytovatele služeb, je přetížení `AddDbContext` , který umožňuje nastavit tuto konfiguraci:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> Doporučujeme vám umožní EF interně spravuje své vlastní služby, pokud nemáte důvod zkombinovat interních EF služeb do vaší aplikace poskytovatele služeb. Hlavní důvod, proč že můžete chtít provést, je váš poskytovatel služeb aplikace použít k nahrazení služeb, které EF používá interně

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>Příkazy DNX = > .NET CLI (platí pouze pro projekty ASP.NET Core)

Pokud jste dříve používali `dnx ef` příkazy pro projekty ASP.NET 5, tyto mají teď na adrese `dotnet ef` příkazy. Podle stejné syntaxe příkazu stále platí. Můžete použít `dotnet ef --help` pro informace o syntaxi.

Způsob, jakým jsou registrovány příkazy změnil ve verzi RC2, z důvodu DNX teď nahrazuje rozhraní příkazového řádku .NET. Příkazy jsou zaregistrovaní v `tools` tématu `project.json`:

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> Pokud používáte sadu Visual Studio, můžete nyní použít Konzola správce balíčků pro spuštění příkazů EF pro projekty ASP.NET Core (to není v RC1 podporována). Budete stále muset zaregistrovat příkazy v `tools` část `project.json` provedete to tak.

## <a name="package-manager-commands-require-powershell-5"></a>Správce balíčků příkazy vyžadují prostředí PowerShell 5

Pokud používáte Entity Framework příkazy v konzole Správce balíčků v sadě Visual Studio, je potřeba zajistit, že máte nainstalovaný PowerShell 5. Toto je dočasný požadavek, který se odebere v další vydané verzi (naleznete v tématu [vydat #5327](https://github.com/aspnet/EntityFramework/issues/5327) další podrobnosti).

## <a name="using-imports-in-projectjson"></a>Použití "importy" v souboru project.json

Některé závislosti EF Core nepodporuje .NET Standard ještě. EF Core v projektech .NET Core a .NET Standard může být nutné přidat "importuje" project.json jako dočasné řešení.

Při přidávání EF, obnovení NuGet se zobrazí tato chybová zpráva:

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

Alternativním řešením je k ručnímu importu přenosné profilu "portable net451 + win8". Tato vynutí NuGet přistupovat ke všem tento binární soubory, které odpovídají to poskytují jako kompatibilní rozhraní .NET Standard, i když nejsou. I když "portable net451 + win8" není 100 % kompatibilní s .NET Standard, je dostatečně kompatibilní pro přechod z PCL do .NET Standard. Importy je možné odebrat, když na EF závislosti nakonec upgradujte na .NET Standard.

Několik architektur lze přidat k "importy" v syntaxi pole. Další importy může být nutné v případě, že přidáte další knihovny do projektu.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

Zobrazit [vydat #5176](https://github.com/aspnet/EntityFramework/issues/5176).
