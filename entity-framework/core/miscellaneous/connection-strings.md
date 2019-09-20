---
title: Připojovací řetězce – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: ed89d6d09b15b0dea7fd8bc3ff3e3f631495ecb7
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149111"
---
# <a name="connection-strings"></a>Připojovací řetězce

Většina poskytovatelů databáze vyžaduje pro připojení k databázi nějakou formu připojovacího řetězce. Někdy tento připojovací řetězec obsahuje citlivé informace, které je třeba chránit. Je také možné, že budete muset změnit připojovací řetězec při přesunu aplikace mezi prostředími, jako je vývoj, testování a produkce.

## <a name="winforms--wpf-applications"></a>Technologie WinForms & aplikací WPF

Aplikace WinForms, WPF a ASP.NET 4 mají vyzkoušený a testovaný vzor připojovacího řetězce. Připojovací řetězec by měl být přidán do souboru App. config aplikace (Web. config, pokud používáte ASP.NET). Pokud váš připojovací řetězec obsahuje citlivé informace, jako je uživatelské jméno a heslo, můžete chránit obsah konfiguračního souboru pomocí [nástroje Správce tajných klíčů](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).

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
> Toto `providerName` nastavení není vyžadováno u EF Core připojovacích řetězců uložených v souboru App. config, protože poskytovatel databáze je nakonfigurován prostřednictvím kódu.

Pak můžete přečíst připojovací řetězec pomocí `ConfigurationManager` rozhraní API v `OnConfiguring` metodě vašeho kontextu. Je možné, že budete muset přidat odkaz na `System.Configuration` sestavení rozhraní, abyste mohli používat toto rozhraní API.

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

Připojovací řetězce v aplikaci UWP jsou obvykle připojení SQLite, které pouze určuje místní název souboru. Obvykle neobsahují citlivé informace a nemusejí být měněny při nasazení aplikace. V takovém případě jsou tyto připojovací řetězce obvykle v kódu ponechány, jak je znázorněno níže. Pokud si přejete přesunout je mimo kód, UWP pro něj podporuje koncept nastavení. Podrobnosti najdete v [části nastavení aplikace v dokumentaci UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) .

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

V ASP.NET Core je konfigurační systém velmi flexibilní a připojovací řetězec mohl být uložen v proměnné prostředí, `appsettings.json`v úložišti tajného klíče uživatele nebo v jiném zdroji konfigurace. Další podrobnosti najdete v [části věnované konfiguraci v dokumentaci k ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) . Následující příklad ukazuje připojovací řetězec uložený v `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Kontext je obvykle nakonfigurovaný v `Startup.cs` rámci s připojovacím řetězcem čteným z konfigurace. Všimněte si `GetConnectionString()` , že metoda hledá hodnotu konfigurace, jejíž klíč `ConnectionStrings:<connection string name>`je. Chcete-li použít tuto metodu rozšíření, je nutné importovat obor názvů [Microsoft. Extensions. Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) .

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
