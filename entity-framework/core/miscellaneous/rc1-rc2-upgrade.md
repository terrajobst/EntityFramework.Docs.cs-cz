---
title: Upgrade z verze EF Core 1,0 RC1 na RC2-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 5300fe459ec2b8ab9bb573c7284b009249071d65
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306457"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>Upgrade z verze EF Core 1,0 RC1 na 1,0 RC2

Tento článek poskytuje pokyny pro přesunutí aplikace vytvořené s balíčky RC1 na RC2.

## <a name="package-names-and-versions"></a>Názvy a verze balíčků

Mezi RC1 a RC2 jsme změnili z "Entity Framework 7" na "Entity Framework Core". Další informace o důvodech změny v [tomto příspěvku získáte Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Z důvodu této změny se naše názvy balíčků změnily `EntityFramework.*` z `Microsoft.EntityFrameworkCore.*` na a z `7.0.0-rc1-final` verze na `1.0.0-rc2-final` (nebo `1.0.0-preview1-final` pro nástroje).

**Bude nutné úplně odebrat balíčky RC1 a pak nainstalovat RC2.** Zde je mapování pro některé běžné balíčky.

| Balíček RC1                                               | RC2 ekvivalent                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final      |
| EntityFramework. SQLite 7.0.0-RC1-Final | Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final      |
| EntityFramework7.Npgsql                   3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres             <to be advised>      |
| EntityFramework.SqlServerCompact35        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final      |
| EntityFramework. inMemory 7.0.0-RC1-Final | Microsoft. EntityFrameworkCore. inMemory 1.0.0-RC2-finální      |
| EntityFramework.IBMDataServer             7.0.0-beta1     | Ještě není k dispozici pro RC2                                            |
| EntityFramework. Commands 7.0.0-RC1-Final | Microsoft. EntityFrameworkCore. Tools 1.0.0-preview1-Final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final      |

## <a name="namespaces"></a>Jmenné prostory

Společně s názvy balíčků se obory názvů `Microsoft.Data.Entity.*` změnily z na `Microsoft.EntityFrameworkCore.*`. Tuto změnu můžete zpracovat pomocí rutiny Find/nahraďte `using Microsoft.Data.Entity`. `using Microsoft.EntityFrameworkCore`

## <a name="table-naming-convention-changes"></a>Změny v konvenci vytváření názvů tabulek

Významnou změnou funkčnosti, kterou jsme zavedli v RC2, bylo použití názvu `DbSet<TEntity>` vlastnosti pro danou entitu, jako je název tabulky, na kterou se mapuje, nikoli jenom jako název třídy. Můžete si přečíst další informace o této změně v [souvisejícím problému s oznámením](https://github.com/aspnet/Announcements/issues/167).

Pro existující aplikace RC1 doporučujeme na začátek `OnModelCreating` metody přidat následující kód, abyste zachovali strategii vytváření názvů RC1:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Pokud chcete přijmout novou strategii vytváření názvů, doporučujeme, abyste úspěšně dokončili zbytek kroků upgradu a pak odebrali kód a vytvořili migraci pro použití přejmenování tabulky.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>Změny AddDbContext/Startup.cs (pouze projekty ASP.NET Core)

V RC1 jste museli přidat Entity Framework Services do poskytovatele aplikační služby – v `Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

V RC2 můžete odebrat volání `AddEntityFramework()`metody, `AddSqlServer()`, atd.:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

Také je nutné přidat konstruktor do odvozeného kontextu, který provede možnosti kontextu a předává je základnímu konstruktoru. To je potřeba proto, že jsme odebrali některé z Scary Magic, které je snuck na pozadí:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>Předání do objektu IServiceProvider

Pokud máte kód RC1, který předává `IServiceProvider` do kontextu, byl nyní přesunut do `DbContextOptions`, nikoli jako parametr samostatného konstruktoru. Použijte `DbContextOptionsBuilder.UseInternalServiceProvider(...)` k nastavení poskytovatele služeb.

### <a name="testing"></a>Testování

Nejběžnějším scénářem je řídit obor databáze inMemory při testování. Příklad toho, jak to dělá s RC2, najdete v článku o aktualizovaném [testování](testing/index.md) .

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Rozpoznávání interních služeb od poskytovatele aplikačních služeb (pouze projekty ASP.NET Core)

Pokud máte aplikaci ASP.NET Core a chcete, aby EF vyřešil interní služby od poskytovatele aplikační služby, existuje přetížení `AddDbContext` , které vám umožní tuto konfiguraci provést:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider));
```

> [!WARNING]  
> Doporučujeme povolit, aby EF mohl interně spravovat vlastní služby, pokud nemáte důvod ke kombinování interních služeb EF s vaším poskytovatelem aplikačních služeb. Hlavní důvod, který byste mohli chtít provést, je použít poskytovatele služby Application Service k nahrazení služeb, které EF používá interně.

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>Příkazy DNX = > rozhraní .NET CLI (pouze projekty ASP.NET Core)

Pokud jste dříve použili `dnx ef` příkazy pro projekty ASP.NET 5, byly nyní přesunuty na `dotnet ef` příkazy. Stejná syntaxe příkazu se pořád používá. Můžete použít `dotnet ef --help` pro informace o syntaxi.

Způsob registrace příkazů se změnil v RC2, protože DNX nahrazuje rozhraní .NET CLI. Příkazy jsou nyní registrovány v `tools` části v `project.json`:

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
> Pokud používáte aplikaci Visual Studio, můžete nyní použít konzolu Správce balíčků ke spuštění příkazů EF pro ASP.NET Core projekty (to se v RC1 nepodporuje). K tomu je stále potřeba zaregistrovat příkazy v `tools` části. `project.json`

## <a name="package-manager-commands-require-powershell-5"></a>Příkazy správce balíčků vyžadují PowerShell 5.

Použijete-li příkazy Entity Framework v konzole správce balíčků v aplikaci Visual Studio, bude nutné zajistit, aby bylo nainstalováno prostředí PowerShell 5. Jedná se o dočasný požadavek, který se v příští verzi odebere (Další informace najdete v tématu [problém #5327](https://github.com/aspnet/EntityFramework/issues/5327) .)

## <a name="using-imports-in-projectjson"></a>Použití "Imports" v Project. JSON

Některé závislosti EF Core .NET Standard ještě nepodporují. EF Core v .NET Standard a projektech .NET Core může vyžadovat přidání "Imports" do Project. JSON jako dočasné alternativní řešení.

Při přidávání EF se tato chybová zpráva zobrazí při obnovení NuGet:

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

Alternativním řešením je ručně importovat přenosný profil "Portable-net451 + Win8". To vynutí, aby NuGet považovala tyto binární soubory, které odpovídají tomuto, jako kompatibilní rozhraní s .NET Standard, i když nejsou. I když "přenosné net451 + Win8" není 100% kompatibilní s .NET Standard, je dostatečně kompatibilní, aby se přechod z PCL na .NET Standard. Importy je možné odebrat, pokud se nakonec upgradují závislosti EF na .NET Standard.

Do syntaxe pole lze přidat více rozhraní do části Imports. Pokud do projektu přidáte další knihovny, může být nutné provést další import.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

Viz [#5176 problému](https://github.com/aspnet/EntityFramework/issues/5176).
