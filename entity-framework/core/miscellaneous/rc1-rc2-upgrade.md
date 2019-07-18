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
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="99c39-102">Upgrade z verze EF Core 1,0 RC1 na 1,0 RC2</span><span class="sxs-lookup"><span data-stu-id="99c39-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="99c39-103">Tento článek poskytuje pokyny pro přesunutí aplikace vytvořené s balíčky RC1 na RC2.</span><span class="sxs-lookup"><span data-stu-id="99c39-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="99c39-104">Názvy a verze balíčků</span><span class="sxs-lookup"><span data-stu-id="99c39-104">Package Names and Versions</span></span>

<span data-ttu-id="99c39-105">Mezi RC1 a RC2 jsme změnili z "Entity Framework 7" na "Entity Framework Core".</span><span class="sxs-lookup"><span data-stu-id="99c39-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="99c39-106">Další informace o důvodech změny v [tomto příspěvku získáte Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="99c39-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="99c39-107">Z důvodu této změny se naše názvy balíčků změnily `EntityFramework.*` z `Microsoft.EntityFrameworkCore.*` na a z `7.0.0-rc1-final` verze na `1.0.0-rc2-final` (nebo `1.0.0-preview1-final` pro nástroje).</span><span class="sxs-lookup"><span data-stu-id="99c39-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="99c39-108">**Bude nutné úplně odebrat balíčky RC1 a pak nainstalovat RC2.**</span><span class="sxs-lookup"><span data-stu-id="99c39-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="99c39-109">Zde je mapování pro některé běžné balíčky.</span><span class="sxs-lookup"><span data-stu-id="99c39-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="99c39-110">Balíček RC1</span><span class="sxs-lookup"><span data-stu-id="99c39-110">RC1 Package</span></span>                                               | <span data-ttu-id="99c39-111">RC2 ekvivalent</span><span class="sxs-lookup"><span data-stu-id="99c39-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="99c39-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="99c39-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="99c39-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="99c39-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="99c39-114">EntityFramework. SQLite 7.0.0-RC1-Final</span><span class="sxs-lookup"><span data-stu-id="99c39-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="99c39-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="99c39-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="99c39-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="99c39-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="99c39-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span><span class="sxs-lookup"><span data-stu-id="99c39-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="99c39-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="99c39-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="99c39-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="99c39-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="99c39-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="99c39-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="99c39-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="99c39-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="99c39-122">EntityFramework. inMemory 7.0.0-RC1-Final</span><span class="sxs-lookup"><span data-stu-id="99c39-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="99c39-123">Microsoft. EntityFrameworkCore. inMemory 1.0.0-RC2-finální</span><span class="sxs-lookup"><span data-stu-id="99c39-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="99c39-124">EntityFramework.IBMDataServer             7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="99c39-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="99c39-125">Ještě není k dispozici pro RC2</span><span class="sxs-lookup"><span data-stu-id="99c39-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="99c39-126">EntityFramework. Commands 7.0.0-RC1-Final</span><span class="sxs-lookup"><span data-stu-id="99c39-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="99c39-127">Microsoft. EntityFrameworkCore. Tools 1.0.0-preview1-Final</span><span class="sxs-lookup"><span data-stu-id="99c39-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="99c39-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="99c39-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="99c39-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="99c39-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="99c39-130">Jmenné prostory</span><span class="sxs-lookup"><span data-stu-id="99c39-130">Namespaces</span></span>

<span data-ttu-id="99c39-131">Společně s názvy balíčků se obory názvů `Microsoft.Data.Entity.*` změnily z na `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="99c39-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="99c39-132">Tuto změnu můžete zpracovat pomocí rutiny Find/nahraďte `using Microsoft.Data.Entity`. `using Microsoft.EntityFrameworkCore`</span><span class="sxs-lookup"><span data-stu-id="99c39-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="99c39-133">Změny v konvenci vytváření názvů tabulek</span><span class="sxs-lookup"><span data-stu-id="99c39-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="99c39-134">Významnou změnou funkčnosti, kterou jsme zavedli v RC2, bylo použití názvu `DbSet<TEntity>` vlastnosti pro danou entitu, jako je název tabulky, na kterou se mapuje, nikoli jenom jako název třídy.</span><span class="sxs-lookup"><span data-stu-id="99c39-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="99c39-135">Můžete si přečíst další informace o této změně v [souvisejícím problému s oznámením](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="99c39-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="99c39-136">Pro existující aplikace RC1 doporučujeme na začátek `OnModelCreating` metody přidat následující kód, abyste zachovali strategii vytváření názvů RC1:</span><span class="sxs-lookup"><span data-stu-id="99c39-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="99c39-137">Pokud chcete přijmout novou strategii vytváření názvů, doporučujeme, abyste úspěšně dokončili zbytek kroků upgradu a pak odebrali kód a vytvořili migraci pro použití přejmenování tabulky.</span><span class="sxs-lookup"><span data-stu-id="99c39-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="99c39-138">Změny AddDbContext/Startup.cs (pouze projekty ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="99c39-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="99c39-139">V RC1 jste museli přidat Entity Framework Services do poskytovatele aplikační služby – v `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="99c39-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="99c39-140">V RC2 můžete odebrat volání `AddEntityFramework()`metody, `AddSqlServer()`, atd.:</span><span class="sxs-lookup"><span data-stu-id="99c39-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="99c39-141">Také je nutné přidat konstruktor do odvozeného kontextu, který provede možnosti kontextu a předává je základnímu konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="99c39-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="99c39-142">To je potřeba proto, že jsme odebrali některé z Scary Magic, které je snuck na pozadí:</span><span class="sxs-lookup"><span data-stu-id="99c39-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="99c39-143">Předání do objektu IServiceProvider</span><span class="sxs-lookup"><span data-stu-id="99c39-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="99c39-144">Pokud máte kód RC1, který předává `IServiceProvider` do kontextu, byl nyní přesunut do `DbContextOptions`, nikoli jako parametr samostatného konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="99c39-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="99c39-145">Použijte `DbContextOptionsBuilder.UseInternalServiceProvider(...)` k nastavení poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="99c39-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="99c39-146">Testování</span><span class="sxs-lookup"><span data-stu-id="99c39-146">Testing</span></span>

<span data-ttu-id="99c39-147">Nejběžnějším scénářem je řídit obor databáze inMemory při testování.</span><span class="sxs-lookup"><span data-stu-id="99c39-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="99c39-148">Příklad toho, jak to dělá s RC2, najdete v článku o aktualizovaném [testování](testing/index.md) .</span><span class="sxs-lookup"><span data-stu-id="99c39-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="99c39-149">Rozpoznávání interních služeb od poskytovatele aplikačních služeb (pouze projekty ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="99c39-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="99c39-150">Pokud máte aplikaci ASP.NET Core a chcete, aby EF vyřešil interní služby od poskytovatele aplikační služby, existuje přetížení `AddDbContext` , které vám umožní tuto konfiguraci provést:</span><span class="sxs-lookup"><span data-stu-id="99c39-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider));
```

> [!WARNING]  
> <span data-ttu-id="99c39-151">Doporučujeme povolit, aby EF mohl interně spravovat vlastní služby, pokud nemáte důvod ke kombinování interních služeb EF s vaším poskytovatelem aplikačních služeb.</span><span class="sxs-lookup"><span data-stu-id="99c39-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="99c39-152">Hlavní důvod, který byste mohli chtít provést, je použít poskytovatele služby Application Service k nahrazení služeb, které EF používá interně.</span><span class="sxs-lookup"><span data-stu-id="99c39-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="99c39-153">Příkazy DNX = > rozhraní .NET CLI (pouze projekty ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="99c39-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="99c39-154">Pokud jste dříve použili `dnx ef` příkazy pro projekty ASP.NET 5, byly nyní přesunuty na `dotnet ef` příkazy.</span><span class="sxs-lookup"><span data-stu-id="99c39-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="99c39-155">Stejná syntaxe příkazu se pořád používá.</span><span class="sxs-lookup"><span data-stu-id="99c39-155">The same command syntax still applies.</span></span> <span data-ttu-id="99c39-156">Můžete použít `dotnet ef --help` pro informace o syntaxi.</span><span class="sxs-lookup"><span data-stu-id="99c39-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="99c39-157">Způsob registrace příkazů se změnil v RC2, protože DNX nahrazuje rozhraní .NET CLI.</span><span class="sxs-lookup"><span data-stu-id="99c39-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="99c39-158">Příkazy jsou nyní registrovány v `tools` části v `project.json`:</span><span class="sxs-lookup"><span data-stu-id="99c39-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

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
> <span data-ttu-id="99c39-159">Pokud používáte aplikaci Visual Studio, můžete nyní použít konzolu Správce balíčků ke spuštění příkazů EF pro ASP.NET Core projekty (to se v RC1 nepodporuje).</span><span class="sxs-lookup"><span data-stu-id="99c39-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="99c39-160">K tomu je stále potřeba zaregistrovat příkazy v `tools` části. `project.json`</span><span class="sxs-lookup"><span data-stu-id="99c39-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="99c39-161">Příkazy správce balíčků vyžadují PowerShell 5.</span><span class="sxs-lookup"><span data-stu-id="99c39-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="99c39-162">Použijete-li příkazy Entity Framework v konzole správce balíčků v aplikaci Visual Studio, bude nutné zajistit, aby bylo nainstalováno prostředí PowerShell 5.</span><span class="sxs-lookup"><span data-stu-id="99c39-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="99c39-163">Jedná se o dočasný požadavek, který se v příští verzi odebere (Další informace najdete v tématu [problém #5327](https://github.com/aspnet/EntityFramework/issues/5327) .)</span><span class="sxs-lookup"><span data-stu-id="99c39-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="99c39-164">Použití "Imports" v Project. JSON</span><span class="sxs-lookup"><span data-stu-id="99c39-164">Using "imports" in project.json</span></span>

<span data-ttu-id="99c39-165">Některé závislosti EF Core .NET Standard ještě nepodporují.</span><span class="sxs-lookup"><span data-stu-id="99c39-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="99c39-166">EF Core v .NET Standard a projektech .NET Core může vyžadovat přidání "Imports" do Project. JSON jako dočasné alternativní řešení.</span><span class="sxs-lookup"><span data-stu-id="99c39-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="99c39-167">Při přidávání EF se tato chybová zpráva zobrazí při obnovení NuGet:</span><span class="sxs-lookup"><span data-stu-id="99c39-167">When adding EF, NuGet restore will display this error message:</span></span>

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

<span data-ttu-id="99c39-168">Alternativním řešením je ručně importovat přenosný profil "Portable-net451 + Win8".</span><span class="sxs-lookup"><span data-stu-id="99c39-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="99c39-169">To vynutí, aby NuGet považovala tyto binární soubory, které odpovídají tomuto, jako kompatibilní rozhraní s .NET Standard, i když nejsou.</span><span class="sxs-lookup"><span data-stu-id="99c39-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="99c39-170">I když "přenosné net451 + Win8" není 100% kompatibilní s .NET Standard, je dostatečně kompatibilní, aby se přechod z PCL na .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="99c39-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="99c39-171">Importy je možné odebrat, pokud se nakonec upgradují závislosti EF na .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="99c39-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="99c39-172">Do syntaxe pole lze přidat více rozhraní do části Imports.</span><span class="sxs-lookup"><span data-stu-id="99c39-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="99c39-173">Pokud do projektu přidáte další knihovny, může být nutné provést další import.</span><span class="sxs-lookup"><span data-stu-id="99c39-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="99c39-174">Viz [#5176 problému](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="99c39-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
