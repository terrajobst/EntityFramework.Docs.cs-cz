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
# <a name="connection-strings"></a><span data-ttu-id="02a70-102">Připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="02a70-102">Connection Strings</span></span>

<span data-ttu-id="02a70-103">Většina poskytovatelů databáze vyžaduje pro připojení k databázi nějakou formu připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="02a70-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="02a70-104">Někdy tento připojovací řetězec obsahuje citlivé informace, které je třeba chránit.</span><span class="sxs-lookup"><span data-stu-id="02a70-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="02a70-105">Je také možné, že budete muset změnit připojovací řetězec při přesunu aplikace mezi prostředími, jako je vývoj, testování a produkce.</span><span class="sxs-lookup"><span data-stu-id="02a70-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="winforms--wpf-applications"></a><span data-ttu-id="02a70-106">Technologie WinForms & aplikací WPF</span><span class="sxs-lookup"><span data-stu-id="02a70-106">WinForms & WPF Applications</span></span>

<span data-ttu-id="02a70-107">Aplikace WinForms, WPF a ASP.NET 4 mají vyzkoušený a testovaný vzor připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="02a70-107">WinForms, WPF, and ASP.NET 4 applications have a tried and tested connection string pattern.</span></span> <span data-ttu-id="02a70-108">Připojovací řetězec by měl být přidán do souboru App. config aplikace (Web. config, pokud používáte ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="02a70-108">The connection string should be added to your application's App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="02a70-109">Pokud váš připojovací řetězec obsahuje citlivé informace, jako je uživatelské jméno a heslo, můžete chránit obsah konfiguračního souboru pomocí [nástroje Správce tajných klíčů](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="02a70-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).</span></span>

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
> <span data-ttu-id="02a70-110">Toto `providerName` nastavení není vyžadováno u EF Core připojovacích řetězců uložených v souboru App. config, protože poskytovatel databáze je nakonfigurován prostřednictvím kódu.</span><span class="sxs-lookup"><span data-stu-id="02a70-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="02a70-111">Pak můžete přečíst připojovací řetězec pomocí `ConfigurationManager` rozhraní API v `OnConfiguring` metodě vašeho kontextu.</span><span class="sxs-lookup"><span data-stu-id="02a70-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="02a70-112">Je možné, že budete muset přidat odkaz na `System.Configuration` sestavení rozhraní, abyste mohli používat toto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="02a70-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="02a70-113">Univerzální platforma Windows (UPW)</span><span class="sxs-lookup"><span data-stu-id="02a70-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="02a70-114">Připojovací řetězce v aplikaci UWP jsou obvykle připojení SQLite, které pouze určuje místní název souboru.</span><span class="sxs-lookup"><span data-stu-id="02a70-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="02a70-115">Obvykle neobsahují citlivé informace a nemusejí být měněny při nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="02a70-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="02a70-116">V takovém případě jsou tyto připojovací řetězce obvykle v kódu ponechány, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="02a70-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="02a70-117">Pokud si přejete přesunout je mimo kód, UWP pro něj podporuje koncept nastavení. Podrobnosti najdete v [části nastavení aplikace v dokumentaci UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) .</span><span class="sxs-lookup"><span data-stu-id="02a70-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

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

## <a name="aspnet-core"></a><span data-ttu-id="02a70-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="02a70-118">ASP.NET Core</span></span>

<span data-ttu-id="02a70-119">V ASP.NET Core je konfigurační systém velmi flexibilní a připojovací řetězec mohl být uložen v proměnné prostředí, `appsettings.json`v úložišti tajného klíče uživatele nebo v jiném zdroji konfigurace.</span><span class="sxs-lookup"><span data-stu-id="02a70-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="02a70-120">Další podrobnosti najdete v [části věnované konfiguraci v dokumentaci k ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) .</span><span class="sxs-lookup"><span data-stu-id="02a70-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="02a70-121">Následující příklad ukazuje připojovací řetězec uložený v `appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="02a70-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="02a70-122">Kontext je obvykle nakonfigurovaný v `Startup.cs` rámci s připojovacím řetězcem čteným z konfigurace.</span><span class="sxs-lookup"><span data-stu-id="02a70-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="02a70-123">Všimněte si `GetConnectionString()` , že metoda hledá hodnotu konfigurace, jejíž klíč `ConnectionStrings:<connection string name>`je.</span><span class="sxs-lookup"><span data-stu-id="02a70-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span> <span data-ttu-id="02a70-124">Chcete-li použít tuto metodu rozšíření, je nutné importovat obor názvů [Microsoft. Extensions. Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) .</span><span class="sxs-lookup"><span data-stu-id="02a70-124">You need to import the [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) namespace to use this extension method.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
