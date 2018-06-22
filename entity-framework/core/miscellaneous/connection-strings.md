---
title: Připojovací řetězce - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: b4ed01f0452d74ac49d3fde780caa5f1b25a6e97
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054097"
---
# <a name="connection-strings"></a>Připojovací řetězce

Většina poskytovatelů databáze vyžadují určitou formu připojovacího řetězce pro připojení k databázi. Někdy tento připojovací řetězec obsahuje citlivé informace, které je potřeba chránit. Můžete také změnit připojovací řetězec, protože mezi prostředími, jako je například vývoj, testování a provozním přesunete vaší aplikace.

## <a name="net-framework-applications"></a>Aplikace rozhraní .NET framework

Aplikace rozhraní .NET framework, například WinForms, WPF, konzoly a technologii ASP.NET 4 mají vzor pokusů a otestovaná připojovací řetězec. Připojovací řetězec musí být přidaní do souboru App.config vaší aplikace (Web.config Pokud používáte ASP.NET). Pokud připojovací řetězec obsahuje citlivé informace, jako je například uživatelské jméno a heslo, budete moci chránit obsah pomocí souboru konfigurace [chráněné konfigurace](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

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
> `providerName` Nastavení není vyžadován na základní EF připojovací řetězce uložené v souboru App.config, protože zprostředkovatel databáze je konfigurován pomocí kódu.

Pak si můžete přečíst, připojovací řetězec pomocí `ConfigurationManager` rozhraní API v váš kontext `OnConfiguring` metoda. Budete muset přidat odkaz na `System.Configuration` sestavení rozhraní, abyste mohli používat toto rozhraní API.

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

## <a name="universal-windows-platform-uwp"></a>Univerzální platformu Windows (UWP)

Připojovací řetězce v aplikaci UWP jsou obvykle SQLite připojení, které právě Určuje název místního souboru. Obvykle neobsahují citlivé informace a není potřeba změnit, protože je aplikace nasazená. Jako takový tyto připojovací řetězce jsou obvykle zůstane v kódu, jak je uvedeno níže. Pokud chcete přesunout je mimo kód pak UWP podporuje koncept nastavení, najdete v článku [části Nastavení aplikace UWP dokumentace](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) podrobnosti.

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

## <a name="aspnet-core"></a>Jádro ASP.NET

V ASP.NET Core konfigurační systém je velmi flexibilní a připojovací řetězec může být uložený v `appsettings.json`, proměnné prostředí, tajný úložiště uživatele nebo jiný zdroj konfigurace. Najdete v článku [konfigurační oddíl dokumentace ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) další podrobnosti. Následující příklad ukazuje připojovací řetězec, který je uložen v `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Kontext je obvykle konfigurovaná `Startup.cs` připojovacím řetězcem, který je čten z konfigurace. Poznámka: `GetConnectionString()` metoda hledá hodnotu konfigurace, jehož klíč je `ConnectionStrings:<connection string name>`.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
