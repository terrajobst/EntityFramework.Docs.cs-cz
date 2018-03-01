---
title: "Upgrade z EF základní 1.0 RC1 na RC2 - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
ms.technology: entity-framework-core
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: e76886729248101ccac024568cf9abcd945fca33
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>Upgrade z EF základní 1.0 RC1 na 1.0 RC2

Tento článek obsahuje pokyny pro přesouvání aplikace vytvořené s nástroji RC1 balíčky, do kterých RC2.

## <a name="package-names-and-versions"></a>Balíček názvy a verze

Mezi RC1 a RC2 jsme změněn z "Entity Framework 7" na "Entity Framework Core". Další informace o důvody pro změnu v hodnotě [tento příspěvek Scotta Hanselmana, kde](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Naše názvy balíčků z důvodu této změny se změnil z `EntityFramework.*` k `Microsoft.EntityFrameworkCore.*` a naše verze `7.0.0-rc1-final` k `1.0.0-rc2-final` (nebo `1.0.0-preview1-final` pro nástrojů).

**Budete muset úplně odebrat balíčky RC1 a pak nainstalujte RC2 těch, které jsou.** Zde je mapování pro některé běžné balíčky.

| Balíček RC1                                               | Ekvivalentní RC2                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final      |
| EntityFramework.SQLite                    7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final      |
| EntityFramework7.Npgsql                   3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres             <to be advised>      |
| EntityFramework.SqlServerCompact35        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final      |
| EntityFramework.InMemory                  7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final      |
| EntityFramework.IBMDataServer             7.0.0-beta1     | Ještě není k dispozici pro RC2                                            |
| EntityFramework.Commands 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final      |

## <a name="namespaces"></a>Jmenné prostory

Obory názvů společně s názvy balíčku, se změnil z `Microsoft.Data.Entity.*` k `Microsoft.EntityFrameworkCore.*`. Dokáže zpracovat tuto změnu s vyhledání a nahrazení z `using Microsoft.Data.Entity` s `using Microsoft.EntityFrameworkCore`.

## <a name="table-naming-convention-changes"></a>Tabulky změn konvence pojmenování

Významné změny funkční vzali jsme RC2 byl použít název `DbSet<TEntity>` vlastnost pro danou entitu jako název tabulky mapuje, ne jenom název třídy. Další informace o této změně v [problém související oznámení](https://github.com/aspnet/Announcements/issues/167).

Pro existující aplikace RC1, doporučujeme, abyste přidáním následující kód do začátku vaší `OnModelCreating` metoda zachovat strategie vytváření názvů RC1:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Pokud chcete přijmout nové strategie vytváření názvů, bychom doporučili úspěšně, že dokončení zbývající kroky upgradu a pak odebrání kódu a vytváření migrace použít v tabulce přejmenuje.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext / Startup.cs změní (pouze projektů ASP.NET Core)

V RC1, jste museli přidání Entity Framework služeb k aplikaci poskytovatele služeb - v `Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

V RC2, můžete odebrat volání `AddEntityFramework()`, `AddSqlServer()`atd.:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

Musíte taky přidejte konstruktor, odvozené kontext, který přebírá kontext možnosti a předá je základní konstruktoru. To je nutné, protože jsme odebrali některé strach magic, který je v snuck na pozadí:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>Předávání IServiceProvider

Pokud máte RC1 kód, který předává `IServiceProvider` do kontextu, to je nyní přesunuta do `DbContextOptions`, místo se parametr samostatné konstruktor. Použití `DbContextOptionsBuilder.UseInternalServiceProvider(...)` nastavení poskytovatele služeb.

### <a name="testing"></a>Testování

Nejběžnější scénáře tohoto postupu se k řízení rozsahu InMemory databáze při testování. V tématu aktualizovaný [testování](testing/index.md) článku příklad to s RC2.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Řešení interních služeb ze stránky ASP (pouze projektů ASP.NET Core)

Pokud máte aplikaci ASP.NET Core a chcete EF k rozpoznávání služeb interní ze zprostředkovatele služeb aplikací, je přetížení `AddDbContext` , můžete konfigurovat toto:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> Doporučujeme vám umožní EF interně Správa vlastní služby, pokud nemáte důvod kombinovat interní EF služby do aplikace poskytovatele služeb. Hlavním důvodem, že můžete k tomu je použít k nahrazení služby, které používá EF interně aplikaci poskytovatele služeb

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>Příkazy DNX = > .NET rozhraní příkazového řádku (pouze projektů ASP.NET Core)

Pokud jste dříve používali `dnx ef` příkazy pro projekty ASP.NET 5, tyto mají nyní přesunuta do `dotnet ef` příkazy. Stále se vztahuje stejnou syntaxi příkazu. Můžete použít `dotnet ef --help` syntaxe informace.

Způsob, jakým jsou registrované příkazy se změnilo v RC2 z důvodu DNX nahrazují .NET rozhraní příkazového řádku. Příkazy v nyní zaregistrováni `tools` kapitoly `project.json`:

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
> Pokud používáte Visual Studio, teď můžete konzolu Správce balíčků ke spuštění příkazů EF pro projekty ASP.NET Core (to není podporováno v RC1). Stále je třeba zaregistrovat příkazy v `tools` části `project.json` k tomu.

## <a name="package-manager-commands-require-powershell-5"></a>Správce balíčků příkazy vyžadují prostředí PowerShell 5

Pokud používáte rozhraní Entity Framework příkazy v konzole Správce balíčků v sadě Visual Studio, budete muset Ujistěte se, že máte nainstalovaný 5 prostředí PowerShell. Toto je dočasný požadavek, který se odebere na další vydání (najdete v části [vydání #5327](https://github.com/aspnet/EntityFramework/issues/5327) další podrobnosti).

## <a name="using-imports-in-projectjson"></a>Použití "importy" v souboru project.json

Některé základní EF závislosti nepodporují .NET Standard ještě. Základní EF v projektech .NET Standard a .NET Core může být nutné přidat "importuje" do souboru project.json to dočasně vyřešit.

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

Alternativní řešení je k ručnímu importu přenosné profilu "přenositelností net451 + win8". Tento vynutí NuGet zacházet s tento binární soubory, které odpovídají to zadejte jako architektura kompatibilní s .NET Standard, i když nejsou. I když "přenositelností net451 + win8" není kompatibilní s .NET standardní 100 %, je dostatečně kompatibilní, aby se pro přechod z PCL .NET Standard. Importy můžete odebrat, pokud je EF závislosti nakonec upgradujte na .NET Standard.

Více rozhraní můžete přidat do "importy" v syntaxi pole. Další importy může být nutné v případě, že do projektu přidejte další knihovny.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

V tématu [vydání #5176](https://github.com/aspnet/EntityFramework/issues/5176).
