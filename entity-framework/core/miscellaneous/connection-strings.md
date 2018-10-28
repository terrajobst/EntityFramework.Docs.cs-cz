---
title: Připojovací řetězce – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: 7bb39d260f700e5087673e92a50377dc68151710
ms.sourcegitcommit: 85ccc9ed42d4aaf7525c6312058c5c9ebdaed3ae
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2018
ms.locfileid: "50191339"
---
# <a name="connection-strings"></a><span data-ttu-id="35270-102">Připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="35270-102">Connection Strings</span></span>

<span data-ttu-id="35270-103">Většina poskytovatelů databáze vyžadují určitou formu připojovací řetězec pro připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="35270-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="35270-104">Někdy tento připojovací řetězec obsahuje citlivé informace, které je potřeba chránit.</span><span class="sxs-lookup"><span data-stu-id="35270-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="35270-105">Také budete muset změnit připojovací řetězec při přesunu mezi prostředími, jako je vývoj, testování a produkční aplikace.</span><span class="sxs-lookup"><span data-stu-id="35270-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="net-framework-applications"></a><span data-ttu-id="35270-106">Aplikace rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="35270-106">.NET Framework Applications</span></span>

<span data-ttu-id="35270-107">Aplikace rozhraní .NET framework, jako je například WinForms, WPF, konzoly a technologii ASP.NET 4 mají Řetězcový vzorek vyzkoušená a otestovaná připojení.</span><span class="sxs-lookup"><span data-stu-id="35270-107">.NET Framework applications, such as WinForms, WPF, Console, and ASP.NET 4, have a tried and tested connection string pattern.</span></span> <span data-ttu-id="35270-108">Připojovací řetězec, měli byste přidat do souboru App.config aplikace (Web.config Pokud používáte ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="35270-108">The connection string should be added to your applications App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="35270-109">Pokud váš připojovací řetězec obsahuje citlivé informace, jako je například uživatelské jméno a heslo, budete moci chránit obsah pomocí souboru konfigurace [Protected Configuration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span><span class="sxs-lookup"><span data-stu-id="35270-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using [Protected Configuration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span></span>

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
> <span data-ttu-id="35270-110">`providerName` Nastavení není vyžadován na EF Core připojovací řetězce, která je uložená v souboru App.config, protože poskytovatel databáze se konfiguruje prostřednictvím kódu.</span><span class="sxs-lookup"><span data-stu-id="35270-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="35270-111">Pak si můžete přečíst, připojovací řetězec pomocí `ConfigurationManager` rozhraní API v objektu context `OnConfiguring` metody.</span><span class="sxs-lookup"><span data-stu-id="35270-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="35270-112">Budete muset přidat odkaz na `System.Configuration` sestavení rozhraní framework bude moct pomocí tohoto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="35270-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="35270-113">Univerzální platforma Windows (UPW)</span><span class="sxs-lookup"><span data-stu-id="35270-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="35270-114">Připojovací řetězce v aplikaci pro UPW se obvykle SQLite připojení, které právě Určuje místní název souboru.</span><span class="sxs-lookup"><span data-stu-id="35270-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="35270-115">Obvykle neobsahují citlivé informace a není potřeba změnit, protože je aplikace nasazená.</span><span class="sxs-lookup"><span data-stu-id="35270-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="35270-116">V důsledku toho tyto řetězce připojení, se obvykle dají zůstat v kódu, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="35270-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="35270-117">Pokud budete chtít přesunout mimo kód, pak UPW podporuje koncept nastavení, najdete v článku [nastavení aplikace část dokumentace k UPW](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="35270-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

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

## <a name="aspnet-core"></a><span data-ttu-id="35270-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="35270-118">ASP.NET Core</span></span>

<span data-ttu-id="35270-119">V ASP.NET Core je velmi flexibilní systém konfigurace, a připojovací řetězec může být uložen v `appsettings.json`, proměnné prostředí, úložiště tajných kódů uživatele nebo jiného zdroje konfigurace.</span><span class="sxs-lookup"><span data-stu-id="35270-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="35270-120">Najdete v článku [konfigurační oddíl dokumentace k ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="35270-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="35270-121">Následující příklad ukazuje připojovacím řetězcem, který je uložen v `appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="35270-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="35270-122">Kontext je typicky nakonfigurován v `Startup.cs` připojovacím řetězcem, který je čten z konfigurace.</span><span class="sxs-lookup"><span data-stu-id="35270-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="35270-123">Poznámka: `GetConnectionString()` metoda hledá hodnotu konfigurace, jehož klíč je `ConnectionStrings:<connection string name>`.</span><span class="sxs-lookup"><span data-stu-id="35270-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span> <span data-ttu-id="35270-124">Je potřeba importovat [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) oboru názvů k použití této metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="35270-124">You need to import the [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) namespace to to use this extension method.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
