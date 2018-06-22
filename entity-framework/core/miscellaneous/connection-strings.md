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
# <a name="connection-strings"></a><span data-ttu-id="70fa4-102">Připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="70fa4-102">Connection Strings</span></span>

<span data-ttu-id="70fa4-103">Většina poskytovatelů databáze vyžadují určitou formu připojovacího řetězce pro připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="70fa4-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="70fa4-104">Někdy tento připojovací řetězec obsahuje citlivé informace, které je potřeba chránit.</span><span class="sxs-lookup"><span data-stu-id="70fa4-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="70fa4-105">Můžete také změnit připojovací řetězec, protože mezi prostředími, jako je například vývoj, testování a provozním přesunete vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="70fa4-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="net-framework-applications"></a><span data-ttu-id="70fa4-106">Aplikace rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="70fa4-106">.NET Framework Applications</span></span>

<span data-ttu-id="70fa4-107">Aplikace rozhraní .NET framework, například WinForms, WPF, konzoly a technologii ASP.NET 4 mají vzor pokusů a otestovaná připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="70fa4-107">.NET Framework applications, such as WinForms, WPF, Console, and ASP.NET 4, have a tried and tested connection string pattern.</span></span> <span data-ttu-id="70fa4-108">Připojovací řetězec musí být přidaní do souboru App.config vaší aplikace (Web.config Pokud používáte ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="70fa4-108">The connection string should be added to your applications App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="70fa4-109">Pokud připojovací řetězec obsahuje citlivé informace, jako je například uživatelské jméno a heslo, budete moci chránit obsah pomocí souboru konfigurace [chráněné konfigurace](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span><span class="sxs-lookup"><span data-stu-id="70fa4-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using [Protected Configuration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span></span>

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
> <span data-ttu-id="70fa4-110">`providerName` Nastavení není vyžadován na základní EF připojovací řetězce uložené v souboru App.config, protože zprostředkovatel databáze je konfigurován pomocí kódu.</span><span class="sxs-lookup"><span data-stu-id="70fa4-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="70fa4-111">Pak si můžete přečíst, připojovací řetězec pomocí `ConfigurationManager` rozhraní API v váš kontext `OnConfiguring` metoda.</span><span class="sxs-lookup"><span data-stu-id="70fa4-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="70fa4-112">Budete muset přidat odkaz na `System.Configuration` sestavení rozhraní, abyste mohli používat toto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="70fa4-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="70fa4-113">Univerzální platformu Windows (UWP)</span><span class="sxs-lookup"><span data-stu-id="70fa4-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="70fa4-114">Připojovací řetězce v aplikaci UWP jsou obvykle SQLite připojení, které právě Určuje název místního souboru.</span><span class="sxs-lookup"><span data-stu-id="70fa4-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="70fa4-115">Obvykle neobsahují citlivé informace a není potřeba změnit, protože je aplikace nasazená.</span><span class="sxs-lookup"><span data-stu-id="70fa4-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="70fa4-116">Jako takový tyto připojovací řetězce jsou obvykle zůstane v kódu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="70fa4-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="70fa4-117">Pokud chcete přesunout je mimo kód pak UWP podporuje koncept nastavení, najdete v článku [části Nastavení aplikace UWP dokumentace](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="70fa4-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

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

## <a name="aspnet-core"></a><span data-ttu-id="70fa4-118">Jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="70fa4-118">ASP.NET Core</span></span>

<span data-ttu-id="70fa4-119">V ASP.NET Core konfigurační systém je velmi flexibilní a připojovací řetězec může být uložený v `appsettings.json`, proměnné prostředí, tajný úložiště uživatele nebo jiný zdroj konfigurace.</span><span class="sxs-lookup"><span data-stu-id="70fa4-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="70fa4-120">Najdete v článku [konfigurační oddíl dokumentace ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="70fa4-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="70fa4-121">Následující příklad ukazuje připojovací řetězec, který je uložen v `appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="70fa4-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="70fa4-122">Kontext je obvykle konfigurovaná `Startup.cs` připojovacím řetězcem, který je čten z konfigurace.</span><span class="sxs-lookup"><span data-stu-id="70fa4-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="70fa4-123">Poznámka: `GetConnectionString()` metoda hledá hodnotu konfigurace, jehož klíč je `ConnectionStrings:<connection string name>`.</span><span class="sxs-lookup"><span data-stu-id="70fa4-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
