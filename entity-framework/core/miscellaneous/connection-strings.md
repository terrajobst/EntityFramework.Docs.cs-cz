---
title: Připojovací řetězce – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: 52a8527170845d3e73ebcec518713ade3f3844f0
ms.sourcegitcommit: 06073f8efde97dd5f540dbfb69f574d8380566fe
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/24/2019
ms.locfileid: "67333844"
---
# <a name="connection-strings"></a>Připojovací řetězce

Většina poskytovatelů databáze vyžadují určitou formu připojovací řetězec pro připojení k databázi. Někdy tento připojovací řetězec obsahuje citlivé informace, které je potřeba chránit. Také budete muset změnit připojovací řetězec při přesunu mezi prostředími, jako je vývoj, testování a produkční aplikace.

## <a name="net-framework-applications"></a>Aplikace rozhraní .NET framework

Aplikace rozhraní .NET framework, jako je například WinForms, WPF, konzoly a technologii ASP.NET 4 mají Řetězcový vzorek vyzkoušená a otestovaná připojení. Připojovací řetězec, měli byste přidat do souboru App.config aplikace (Web.config Pokud používáte ASP.NET). Pokud váš připojovací řetězec obsahuje citlivé informace, jako je například uživatelské jméno a heslo, budete moci chránit obsah pomocí souboru konfigurace [Protected Configuration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>

  <connectionStrings>
    <add name="BloggingDatabase"
         connectionString="Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" />
  </connectionStrings>
</configuration>
```

> [!TIP]  
> `providerName` Nastavení není vyžadován na EF Core připojovací řetězce, která je uložená v souboru App.config, protože poskytovatel databáze se konfiguruje prostřednictvím kódu.

Pak si můžete přečíst, připojovací řetězec pomocí `ConfigurationManager` rozhraní API v objektu context `OnConfiguring` metody. Budete muset přidat odkaz na `System.Configuration` sestavení rozhraní framework bude moct pomocí tohoto rozhraní API.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="universal-windows-platform-uwp"></a>Univerzální platforma Windows (UPW)

Připojovací řetězce v aplikaci pro UPW se obvykle SQLite připojení, které právě Určuje místní název souboru. Obvykle neobsahují citlivé informace a není potřeba změnit, protože je aplikace nasazená. V důsledku toho tyto řetězce připojení, se obvykle dají zůstat v kódu, jak je znázorněno níže. Pokud budete chtít přesunout mimo kód, pak UPW podporuje koncept nastavení, najdete v článku [nastavení aplikace část dokumentace k UPW](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) podrobnosti.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
            optionsBuilder.UseSqlite("Data Source=blogging.db");
    }
}
```

## <a name="aspnet-core"></a>ASP.NET Core

V ASP.NET Core je velmi flexibilní systém konfigurace, a připojovací řetězec může být uložen v `appsettings.json`, proměnné prostředí, úložiště tajných kódů uživatele nebo jiného zdroje konfigurace. Najdete v článku [konfigurační oddíl dokumentace k ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) další podrobnosti. Následující příklad ukazuje připojovacím řetězcem, který je uložen v `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Kontext je typicky nakonfigurován v `Startup.cs` připojovacím řetězcem, který je čten z konfigurace. Poznámka: `GetConnectionString()` metoda hledá hodnotu konfigurace, jehož klíč je `ConnectionStrings:<connection string name>`. Je potřeba importovat [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) obor názvů, aby používali tuto metodu rozšíření.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
