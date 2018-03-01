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
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="e49a0-102">Upgrade z EF základní 1.0 RC1 na 1.0 RC2</span><span class="sxs-lookup"><span data-stu-id="e49a0-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="e49a0-103">Tento článek obsahuje pokyny pro přesouvání aplikace vytvořené s nástroji RC1 balíčky, do kterých RC2.</span><span class="sxs-lookup"><span data-stu-id="e49a0-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="e49a0-104">Balíček názvy a verze</span><span class="sxs-lookup"><span data-stu-id="e49a0-104">Package Names and Versions</span></span>

<span data-ttu-id="e49a0-105">Mezi RC1 a RC2 jsme změněn z "Entity Framework 7" na "Entity Framework Core".</span><span class="sxs-lookup"><span data-stu-id="e49a0-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="e49a0-106">Další informace o důvody pro změnu v hodnotě [tento příspěvek Scotta Hanselmana, kde](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="e49a0-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="e49a0-107">Naše názvy balíčků z důvodu této změny se změnil z `EntityFramework.*` k `Microsoft.EntityFrameworkCore.*` a naše verze `7.0.0-rc1-final` k `1.0.0-rc2-final` (nebo `1.0.0-preview1-final` pro nástrojů).</span><span class="sxs-lookup"><span data-stu-id="e49a0-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="e49a0-108">**Budete muset úplně odebrat balíčky RC1 a pak nainstalujte RC2 těch, které jsou.**</span><span class="sxs-lookup"><span data-stu-id="e49a0-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="e49a0-109">Zde je mapování pro některé běžné balíčky.</span><span class="sxs-lookup"><span data-stu-id="e49a0-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="e49a0-110">Balíček RC1</span><span class="sxs-lookup"><span data-stu-id="e49a0-110">RC1 Package</span></span>                                               | <span data-ttu-id="e49a0-111">Ekvivalentní RC2</span><span class="sxs-lookup"><span data-stu-id="e49a0-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="e49a0-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="e49a0-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="e49a0-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="e49a0-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="e49a0-114">EntityFramework.SQLite                    7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="e49a0-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="e49a0-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="e49a0-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="e49a0-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="e49a0-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="e49a0-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span><span class="sxs-lookup"><span data-stu-id="e49a0-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="e49a0-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="e49a0-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="e49a0-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="e49a0-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="e49a0-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="e49a0-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="e49a0-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="e49a0-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="e49a0-122">EntityFramework.InMemory                  7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="e49a0-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="e49a0-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="e49a0-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="e49a0-124">EntityFramework.IBMDataServer             7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="e49a0-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="e49a0-125">Ještě není k dispozici pro RC2</span><span class="sxs-lookup"><span data-stu-id="e49a0-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="e49a0-126">EntityFramework.Commands 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="e49a0-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="e49a0-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="e49a0-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="e49a0-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="e49a0-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="e49a0-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="e49a0-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="e49a0-130">Jmenné prostory</span><span class="sxs-lookup"><span data-stu-id="e49a0-130">Namespaces</span></span>

<span data-ttu-id="e49a0-131">Obory názvů společně s názvy balíčku, se změnil z `Microsoft.Data.Entity.*` k `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="e49a0-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="e49a0-132">Dokáže zpracovat tuto změnu s vyhledání a nahrazení z `using Microsoft.Data.Entity` s `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="e49a0-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="e49a0-133">Tabulky změn konvence pojmenování</span><span class="sxs-lookup"><span data-stu-id="e49a0-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="e49a0-134">Významné změny funkční vzali jsme RC2 byl použít název `DbSet<TEntity>` vlastnost pro danou entitu jako název tabulky mapuje, ne jenom název třídy.</span><span class="sxs-lookup"><span data-stu-id="e49a0-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="e49a0-135">Další informace o této změně v [problém související oznámení](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="e49a0-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="e49a0-136">Pro existující aplikace RC1, doporučujeme, abyste přidáním následující kód do začátku vaší `OnModelCreating` metoda zachovat strategie vytváření názvů RC1:</span><span class="sxs-lookup"><span data-stu-id="e49a0-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="e49a0-137">Pokud chcete přijmout nové strategie vytváření názvů, bychom doporučili úspěšně, že dokončení zbývající kroky upgradu a pak odebrání kódu a vytváření migrace použít v tabulce přejmenuje.</span><span class="sxs-lookup"><span data-stu-id="e49a0-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="e49a0-138">AddDbContext / Startup.cs změní (pouze projektů ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="e49a0-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="e49a0-139">V RC1, jste museli přidání Entity Framework služeb k aplikaci poskytovatele služeb - v `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="e49a0-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="e49a0-140">V RC2, můžete odebrat volání `AddEntityFramework()`, `AddSqlServer()`atd.:</span><span class="sxs-lookup"><span data-stu-id="e49a0-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="e49a0-141">Musíte taky přidejte konstruktor, odvozené kontext, který přebírá kontext možnosti a předá je základní konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="e49a0-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="e49a0-142">To je nutné, protože jsme odebrali některé strach magic, který je v snuck na pozadí:</span><span class="sxs-lookup"><span data-stu-id="e49a0-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="e49a0-143">Předávání IServiceProvider</span><span class="sxs-lookup"><span data-stu-id="e49a0-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="e49a0-144">Pokud máte RC1 kód, který předává `IServiceProvider` do kontextu, to je nyní přesunuta do `DbContextOptions`, místo se parametr samostatné konstruktor.</span><span class="sxs-lookup"><span data-stu-id="e49a0-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="e49a0-145">Použití `DbContextOptionsBuilder.UseInternalServiceProvider(...)` nastavení poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="e49a0-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="e49a0-146">Testování</span><span class="sxs-lookup"><span data-stu-id="e49a0-146">Testing</span></span>

<span data-ttu-id="e49a0-147">Nejběžnější scénáře tohoto postupu se k řízení rozsahu InMemory databáze při testování.</span><span class="sxs-lookup"><span data-stu-id="e49a0-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="e49a0-148">V tématu aktualizovaný [testování](testing/index.md) článku příklad to s RC2.</span><span class="sxs-lookup"><span data-stu-id="e49a0-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="e49a0-149">Řešení interních služeb ze stránky ASP (pouze projektů ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="e49a0-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="e49a0-150">Pokud máte aplikaci ASP.NET Core a chcete EF k rozpoznávání služeb interní ze zprostředkovatele služeb aplikací, je přetížení `AddDbContext` , můžete konfigurovat toto:</span><span class="sxs-lookup"><span data-stu-id="e49a0-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> <span data-ttu-id="e49a0-151">Doporučujeme vám umožní EF interně Správa vlastní služby, pokud nemáte důvod kombinovat interní EF služby do aplikace poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="e49a0-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="e49a0-152">Hlavním důvodem, že můžete k tomu je použít k nahrazení služby, které používá EF interně aplikaci poskytovatele služeb</span><span class="sxs-lookup"><span data-stu-id="e49a0-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="e49a0-153">Příkazy DNX = > .NET rozhraní příkazového řádku (pouze projektů ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="e49a0-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="e49a0-154">Pokud jste dříve používali `dnx ef` příkazy pro projekty ASP.NET 5, tyto mají nyní přesunuta do `dotnet ef` příkazy.</span><span class="sxs-lookup"><span data-stu-id="e49a0-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="e49a0-155">Stále se vztahuje stejnou syntaxi příkazu.</span><span class="sxs-lookup"><span data-stu-id="e49a0-155">The same command syntax still applies.</span></span> <span data-ttu-id="e49a0-156">Můžete použít `dotnet ef --help` syntaxe informace.</span><span class="sxs-lookup"><span data-stu-id="e49a0-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="e49a0-157">Způsob, jakým jsou registrované příkazy se změnilo v RC2 z důvodu DNX nahrazují .NET rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e49a0-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="e49a0-158">Příkazy v nyní zaregistrováni `tools` kapitoly `project.json`:</span><span class="sxs-lookup"><span data-stu-id="e49a0-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

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
> <span data-ttu-id="e49a0-159">Pokud používáte Visual Studio, teď můžete konzolu Správce balíčků ke spuštění příkazů EF pro projekty ASP.NET Core (to není podporováno v RC1).</span><span class="sxs-lookup"><span data-stu-id="e49a0-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="e49a0-160">Stále je třeba zaregistrovat příkazy v `tools` části `project.json` k tomu.</span><span class="sxs-lookup"><span data-stu-id="e49a0-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="e49a0-161">Správce balíčků příkazy vyžadují prostředí PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="e49a0-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="e49a0-162">Pokud používáte rozhraní Entity Framework příkazy v konzole Správce balíčků v sadě Visual Studio, budete muset Ujistěte se, že máte nainstalovaný 5 prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e49a0-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="e49a0-163">Toto je dočasný požadavek, který se odebere na další vydání (najdete v části [vydání #5327](https://github.com/aspnet/EntityFramework/issues/5327) další podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="e49a0-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="e49a0-164">Použití "importy" v souboru project.json</span><span class="sxs-lookup"><span data-stu-id="e49a0-164">Using "imports" in project.json</span></span>

<span data-ttu-id="e49a0-165">Některé základní EF závislosti nepodporují .NET Standard ještě.</span><span class="sxs-lookup"><span data-stu-id="e49a0-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="e49a0-166">Základní EF v projektech .NET Standard a .NET Core může být nutné přidat "importuje" do souboru project.json to dočasně vyřešit.</span><span class="sxs-lookup"><span data-stu-id="e49a0-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="e49a0-167">Při přidávání EF, obnovení NuGet se zobrazí tato chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="e49a0-167">When adding EF, NuGet restore will display this error message:</span></span>

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

<span data-ttu-id="e49a0-168">Alternativní řešení je k ručnímu importu přenosné profilu "přenositelností net451 + win8".</span><span class="sxs-lookup"><span data-stu-id="e49a0-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="e49a0-169">Tento vynutí NuGet zacházet s tento binární soubory, které odpovídají to zadejte jako architektura kompatibilní s .NET Standard, i když nejsou.</span><span class="sxs-lookup"><span data-stu-id="e49a0-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="e49a0-170">I když "přenositelností net451 + win8" není kompatibilní s .NET standardní 100 %, je dostatečně kompatibilní, aby se pro přechod z PCL .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="e49a0-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="e49a0-171">Importy můžete odebrat, pokud je EF závislosti nakonec upgradujte na .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="e49a0-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="e49a0-172">Více rozhraní můžete přidat do "importy" v syntaxi pole.</span><span class="sxs-lookup"><span data-stu-id="e49a0-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="e49a0-173">Další importy může být nutné v případě, že do projektu přidejte další knihovny.</span><span class="sxs-lookup"><span data-stu-id="e49a0-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="e49a0-174">V tématu [vydání #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="e49a0-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
