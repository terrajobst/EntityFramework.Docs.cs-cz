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
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="54435-102">Upgrade z verze EF Core 1.0 RC1 na 1.0 RC2</span><span class="sxs-lookup"><span data-stu-id="54435-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="54435-103">Tento článek obsahuje pokyny pro přesouvání aplikace sestavené s balíčky RC1 na RC2.</span><span class="sxs-lookup"><span data-stu-id="54435-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="54435-104">Názvy balíčků a verze</span><span class="sxs-lookup"><span data-stu-id="54435-104">Package Names and Versions</span></span>

<span data-ttu-id="54435-105">Mezi RC1 a RC2 jsme změní z "Entity Frameworku 7" na "Entity Framework Core".</span><span class="sxs-lookup"><span data-stu-id="54435-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="54435-106">Další informace o důvodech, proč pro změnu v hodnotě [tento příspěvek Scotta Hanselmana,](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="54435-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="54435-107">Z důvodu této změny změnit náš názvy balíčků z `EntityFramework.*` k `Microsoft.EntityFrameworkCore.*` a naše verze `7.0.0-rc1-final` k `1.0.0-rc2-final` (nebo `1.0.0-preview1-final` nástrojů pro).</span><span class="sxs-lookup"><span data-stu-id="54435-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="54435-108">**Budete muset úplně odebrat RC1 balíčky a pak nainstalujte RC2 těch, které jsou.**</span><span class="sxs-lookup"><span data-stu-id="54435-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="54435-109">Tady je mapování pro některé běžné balíčky.</span><span class="sxs-lookup"><span data-stu-id="54435-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="54435-110">Balíček RC1</span><span class="sxs-lookup"><span data-stu-id="54435-110">RC1 Package</span></span>                                               | <span data-ttu-id="54435-111">Ekvivalentní verzi RC2</span><span class="sxs-lookup"><span data-stu-id="54435-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="54435-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="54435-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="54435-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="54435-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="54435-114">EntityFramework.SQLite 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="54435-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="54435-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="54435-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="54435-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="54435-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="54435-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span><span class="sxs-lookup"><span data-stu-id="54435-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="54435-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="54435-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="54435-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="54435-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="54435-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="54435-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="54435-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="54435-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="54435-122">EntityFramework.InMemory 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="54435-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="54435-123">Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="54435-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="54435-124">EntityFramework.IBMDataServer             7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="54435-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="54435-125">Zatím není k dispozici pro RC2</span><span class="sxs-lookup"><span data-stu-id="54435-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="54435-126">EntityFramework.Commands 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="54435-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="54435-127">Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="54435-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="54435-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="54435-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="54435-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="54435-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="54435-130">Jmenné prostory</span><span class="sxs-lookup"><span data-stu-id="54435-130">Namespaces</span></span>

<span data-ttu-id="54435-131">Spolu s názvy balíčků, obory názvů změnil z `Microsoft.Data.Entity.*` k `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="54435-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="54435-132">Tato změna se najít/nahradit aplikace dokáže zpracovat `using Microsoft.Data.Entity` s `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="54435-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="54435-133">Tabulky změn konvence názvů</span><span class="sxs-lookup"><span data-stu-id="54435-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="54435-134">Významné funkční změny jsme provedli v RC2 byl určený název `DbSet<TEntity>` vlastností pro danou entitu jako název tabulky je namapován na, ne jenom název třídy.</span><span class="sxs-lookup"><span data-stu-id="54435-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="54435-135">Další informace o této změně v [problém související oznámení](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="54435-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="54435-136">Pro existující aplikace RC1, doporučujeme přidat následující kód na začátek vašeho `OnModelCreating` metoda zachovat RC1 taková strategie:</span><span class="sxs-lookup"><span data-stu-id="54435-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="54435-137">Pokud chcete přijmout nové strategie vytváření názvů, by doporučujeme úspěšně vyplnění zbývající kroky upgradu a pak odstranění kódu a vytváření migrace použít v tabulce přejmenuje.</span><span class="sxs-lookup"><span data-stu-id="54435-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="54435-138">AddDbContext / Startup.cs změní (platí pouze pro projekty ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="54435-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="54435-139">V RC1, museli jste přidání Entity Framework služeb k aplikaci poskytovatele služeb – v `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="54435-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="54435-140">Ve verzi RC2, můžete odebrat volání `AddEntityFramework()`, `AddSqlServer()`atd.:</span><span class="sxs-lookup"><span data-stu-id="54435-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="54435-141">Také je potřeba přidat konstruktoru, do vaší odvozené kontext, který přebírá kontext možnosti a předává je do konstruktor základní třídy.</span><span class="sxs-lookup"><span data-stu-id="54435-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="54435-142">To je potřeba, proto jsme odstranili některý scary magic, který je v snuck na pozadí:</span><span class="sxs-lookup"><span data-stu-id="54435-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="54435-143">Předávání IServiceProvider</span><span class="sxs-lookup"><span data-stu-id="54435-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="54435-144">Máte RC1 kód, který se předává `IServiceProvider` ke kontextu, to se teď přesunul na `DbContextOptions`, namísto parametru samostatné konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="54435-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="54435-145">Použití `DbContextOptionsBuilder.UseInternalServiceProvider(...)` nastavení poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="54435-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="54435-146">Testování</span><span class="sxs-lookup"><span data-stu-id="54435-146">Testing</span></span>

<span data-ttu-id="54435-147">Nejběžnější scénář to bylo řízení rozsahu databázi InMemory při testování.</span><span class="sxs-lookup"><span data-stu-id="54435-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="54435-148">Zobrazit aktualizovaný [testování](testing/index.md) článku příklad to RC2.</span><span class="sxs-lookup"><span data-stu-id="54435-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="54435-149">Řešení interních služeb z aplikace poskytovatele služeb (platí pouze pro projekty ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="54435-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="54435-150">Pokud máte aplikace ASP.NET Core a EF vyřešit interních služeb z aplikace poskytovatele služeb, je přetížení `AddDbContext` , který umožňuje nastavit tuto konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="54435-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> <span data-ttu-id="54435-151">Doporučujeme vám umožní EF interně spravuje své vlastní služby, pokud nemáte důvod zkombinovat interních EF služeb do vaší aplikace poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="54435-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="54435-152">Hlavní důvod, proč že můžete chtít provést, je váš poskytovatel služeb aplikace použít k nahrazení služeb, které EF používá interně</span><span class="sxs-lookup"><span data-stu-id="54435-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="54435-153">Příkazy DNX = > .NET CLI (platí pouze pro projekty ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="54435-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="54435-154">Pokud jste dříve používali `dnx ef` příkazy pro projekty ASP.NET 5, tyto mají teď na adrese `dotnet ef` příkazy.</span><span class="sxs-lookup"><span data-stu-id="54435-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="54435-155">Podle stejné syntaxe příkazu stále platí.</span><span class="sxs-lookup"><span data-stu-id="54435-155">The same command syntax still applies.</span></span> <span data-ttu-id="54435-156">Můžete použít `dotnet ef --help` pro informace o syntaxi.</span><span class="sxs-lookup"><span data-stu-id="54435-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="54435-157">Způsob, jakým jsou registrovány příkazy změnil ve verzi RC2, z důvodu DNX teď nahrazuje rozhraní příkazového řádku .NET.</span><span class="sxs-lookup"><span data-stu-id="54435-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="54435-158">Příkazy jsou zaregistrovaní v `tools` tématu `project.json`:</span><span class="sxs-lookup"><span data-stu-id="54435-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

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
> <span data-ttu-id="54435-159">Pokud používáte sadu Visual Studio, můžete nyní použít Konzola správce balíčků pro spuštění příkazů EF pro projekty ASP.NET Core (to není v RC1 podporována).</span><span class="sxs-lookup"><span data-stu-id="54435-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="54435-160">Budete stále muset zaregistrovat příkazy v `tools` část `project.json` provedete to tak.</span><span class="sxs-lookup"><span data-stu-id="54435-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="54435-161">Správce balíčků příkazy vyžadují prostředí PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="54435-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="54435-162">Pokud používáte Entity Framework příkazy v konzole Správce balíčků v sadě Visual Studio, je potřeba zajistit, že máte nainstalovaný PowerShell 5.</span><span class="sxs-lookup"><span data-stu-id="54435-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="54435-163">Toto je dočasný požadavek, který se odebere v další vydané verzi (naleznete v tématu [vydat #5327](https://github.com/aspnet/EntityFramework/issues/5327) další podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="54435-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="54435-164">Použití "importy" v souboru project.json</span><span class="sxs-lookup"><span data-stu-id="54435-164">Using "imports" in project.json</span></span>

<span data-ttu-id="54435-165">Některé závislosti EF Core nepodporuje .NET Standard ještě.</span><span class="sxs-lookup"><span data-stu-id="54435-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="54435-166">EF Core v projektech .NET Core a .NET Standard může být nutné přidat "importuje" project.json jako dočasné řešení.</span><span class="sxs-lookup"><span data-stu-id="54435-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="54435-167">Při přidávání EF, obnovení NuGet se zobrazí tato chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="54435-167">When adding EF, NuGet restore will display this error message:</span></span>

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

<span data-ttu-id="54435-168">Alternativním řešením je k ručnímu importu přenosné profilu "portable net451 + win8".</span><span class="sxs-lookup"><span data-stu-id="54435-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="54435-169">Tato vynutí NuGet přistupovat ke všem tento binární soubory, které odpovídají to poskytují jako kompatibilní rozhraní .NET Standard, i když nejsou.</span><span class="sxs-lookup"><span data-stu-id="54435-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="54435-170">I když "portable net451 + win8" není 100 % kompatibilní s .NET Standard, je dostatečně kompatibilní pro přechod z PCL do .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="54435-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="54435-171">Importy je možné odebrat, když na EF závislosti nakonec upgradujte na .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="54435-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="54435-172">Několik architektur lze přidat k "importy" v syntaxi pole.</span><span class="sxs-lookup"><span data-stu-id="54435-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="54435-173">Další importy může být nutné v případě, že přidáte další knihovny do projektu.</span><span class="sxs-lookup"><span data-stu-id="54435-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="54435-174">Zobrazit [vydat #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="54435-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
