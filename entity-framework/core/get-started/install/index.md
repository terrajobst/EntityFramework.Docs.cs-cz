---
title: Instalace Entity Framework Core-EF Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 6575b1ac028f8b67b49ca7f4e49d6f19500be98f
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136175"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="01220-102">Instalace Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="01220-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01220-103">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="01220-103">Prerequisites</span></span>

* <span data-ttu-id="01220-104">EF Core je knihovna [.NET Standard 2,0](/dotnet/standard/net-standard) .</span><span class="sxs-lookup"><span data-stu-id="01220-104">EF Core is a [.NET Standard 2.0](/dotnet/standard/net-standard) library.</span></span> <span data-ttu-id="01220-105">Proto EF Core vyžaduje implementaci rozhraní .NET, která podporuje spuštění .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="01220-105">So EF Core requires a .NET implementation that supports .NET Standard 2.0 to run.</span></span> <span data-ttu-id="01220-106">Na EF Core můžou být taky odkazovány jinými knihovnami .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="01220-106">EF Core can also be referenced by other .NET Standard 2.0 libraries.</span></span>

* <span data-ttu-id="01220-107">Můžete například použít EF Core pro vývoj aplikací, které cílí na .NET Core.</span><span class="sxs-lookup"><span data-stu-id="01220-107">For example, you can use EF Core to develop apps that target .NET Core.</span></span> <span data-ttu-id="01220-108">Sestavování aplikací .NET Core vyžaduje [.NET Core SDK](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="01220-108">Building .NET Core apps requires the [.NET Core SDK](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="01220-109">Volitelně můžete také použít vývojové prostředí, jako je [Visual Studio](https://visualstudio.microsoft.com/vs), [Visual Studio pro Mac](https://visualstudio.microsoft.com/vs/mac)nebo [Visual Studio Code](https://code.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="01220-109">Optionally, you can also use a development environment like [Visual Studio](https://visualstudio.microsoft.com/vs), [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac), or [Visual Studio Code](https://code.visualstudio.com).</span></span> <span data-ttu-id="01220-110">Další informace najdete v [Začínáme pomocí .NET Core](/dotnet/core/get-started).</span><span class="sxs-lookup"><span data-stu-id="01220-110">For more information, check [Getting Started with .NET Core](/dotnet/core/get-started).</span></span>

* <span data-ttu-id="01220-111">Můžete použít EF Core pro vývoj aplikací v systému Windows pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01220-111">You can use EF Core to develop applications on Windows using Visual Studio.</span></span> <span data-ttu-id="01220-112">Doporučuje se nejnovější verze sady [Visual Studio](https://visualstudio.microsoft.com/vs) .</span><span class="sxs-lookup"><span data-stu-id="01220-112">The latest version of [Visual Studio](https://visualstudio.microsoft.com/vs) is recommended.</span></span>

* <span data-ttu-id="01220-113">EF Core lze spustit na jiných implementacích rozhraní .NET, jako je [Xamarin](https://dotnet.microsoft.com/apps/xamarin) a .NET Native.</span><span class="sxs-lookup"><span data-stu-id="01220-113">EF Core can run on other .NET implementations like [Xamarin](https://dotnet.microsoft.com/apps/xamarin) and .NET Native.</span></span> <span data-ttu-id="01220-114">V praxi ale tyto implementace mají omezení za běhu, což může mít vliv na to, jak dobře EF Core fungují ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="01220-114">But in practice those implementations have runtime limitations that may affect how well EF Core works on your app.</span></span> <span data-ttu-id="01220-115">Další informace najdete v tématu [implementace rozhraní .NET podporované nástrojem EF Core](xref:core/platforms/index).</span><span class="sxs-lookup"><span data-stu-id="01220-115">For more information, see [.NET implementations supported by EF Core](xref:core/platforms/index).</span></span>

* <span data-ttu-id="01220-116">Nakonec různí poskytovatelé databáze můžou vyžadovat konkrétní verze databázového stroje, implementace .NET nebo operační systémy.</span><span class="sxs-lookup"><span data-stu-id="01220-116">Finally, different database providers may require specific database engine versions, .NET implementations, or operating systems.</span></span> <span data-ttu-id="01220-117">Ujistěte se, že je k dispozici [poskytovatel databáze EF Core](xref:core/providers/index) , který podporuje správné prostředí pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="01220-117">Make sure an [EF Core database provider](xref:core/providers/index) is available that supports the right environment for your application.</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="01220-118">Získat modul runtime Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="01220-118">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="01220-119">Chcete-li přidat EF Core do aplikace, nainstalujte balíček NuGet pro poskytovatele databáze, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="01220-119">To add EF Core to an application, install the NuGet package for the database provider you want to use.</span></span>

<span data-ttu-id="01220-120">Pokud vytváříte aplikaci ASP.NET Core, nemusíte instalovat poskytovatele v paměti a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="01220-120">If you're building an ASP.NET Core application, you don't need to install the in-memory and SQL Server providers.</span></span> <span data-ttu-id="01220-121">Tito poskytovatelé jsou zahrnutí v aktuálních verzích ASP.NET Core společně s modulem EF Core Runtime.</span><span class="sxs-lookup"><span data-stu-id="01220-121">Those providers are included in current versions of ASP.NET Core, alongside the EF Core runtime.</span></span>  

<span data-ttu-id="01220-122">Chcete-li nainstalovat nebo aktualizovat balíčky NuGet, můžete použít rozhraní příkazového řádku (CLI) .NET Core, dialogové okno Správce balíčků sady Visual Studio nebo konzolu správce sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01220-122">To install or update NuGet packages, you can use the .NET Core command-line interface (CLI), the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

### <a name="net-core-cli"></a><span data-ttu-id="01220-123">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="01220-123">.NET Core CLI</span></span>

* <span data-ttu-id="01220-124">Chcete-li nainstalovat nebo aktualizovat poskytovatele služby EF Core SQL Server, použijte následující příkaz .NET Core CLI z příkazového řádku operačního systému:</span><span class="sxs-lookup"><span data-stu-id="01220-124">Use the following .NET Core CLI command from the operating system's command line to install or update the EF Core SQL Server provider:</span></span>

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* <span data-ttu-id="01220-125">Konkrétní verzi můžete určit v příkazu `dotnet add package` pomocí modifikátoru `-v`.</span><span class="sxs-lookup"><span data-stu-id="01220-125">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="01220-126">Například pro instalaci balíčků EF Core 2.2.0 přidejte `-v 2.2.0` k příkazu.</span><span class="sxs-lookup"><span data-stu-id="01220-126">For example, to install EF Core 2.2.0 packages, append `-v 2.2.0` to the command.</span></span>

<span data-ttu-id="01220-127">Další informace najdete v tématu [nástroje rozhraní příkazového řádku .NET (CLI)](/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="01220-127">For more information, see [.NET command-line interface (CLI) tools](/dotnet/core/tools/).</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="01220-128">Dialogové okno Správce balíčků NuGet sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01220-128">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="01220-129">V nabídce sady Visual Studio vyberte **projekt > spravovat balíčky NuGet**</span><span class="sxs-lookup"><span data-stu-id="01220-129">From the Visual Studio menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="01220-130">Klikněte na kartu **Procházet** nebo **aktualizace** .</span><span class="sxs-lookup"><span data-stu-id="01220-130">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="01220-131">Pokud chcete nainstalovat nebo aktualizovat poskytovatele SQL Server, vyberte balíček `Microsoft.EntityFrameworkCore.SqlServer` a potvrďte ho.</span><span class="sxs-lookup"><span data-stu-id="01220-131">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="01220-132">Další informace najdete v [dialogovém okně Správce balíčků NuGet](/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="01220-132">For more information, see [NuGet Package Manager Dialog](/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="01220-133">Konzola správce balíčků NuGet sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01220-133">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="01220-134">V nabídce aplikace Visual Studio vyberte **nástroje > správce balíčků NuGet > konzola správce balíčků** .</span><span class="sxs-lookup"><span data-stu-id="01220-134">From the Visual Studio menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="01220-135">Chcete-li nainstalovat poskytovatele SQL Server, spusťte následující příkaz v konzole správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="01220-135">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```

* <span data-ttu-id="01220-136">Chcete-li aktualizovat poskytovatele, použijte příkaz `Update-Package`.</span><span class="sxs-lookup"><span data-stu-id="01220-136">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="01220-137">K určení konkrétní verze použijte modifikátor `-Version`.</span><span class="sxs-lookup"><span data-stu-id="01220-137">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="01220-138">Například pro instalaci balíčků EF Core 2.2.0 přidejte `-Version 2.2.0` k příkazům</span><span class="sxs-lookup"><span data-stu-id="01220-138">For example, to install EF Core 2.2.0 packages, append `-Version 2.2.0` to the commands</span></span>

<span data-ttu-id="01220-139">Další informace najdete v tématu [Konzola správce balíčků](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="01220-139">For more information, see [Package Manager Console](/nuget/tools/package-manager-console).</span></span>

## <a name="get-the-entity-framework-core-tools"></a><span data-ttu-id="01220-140">Získat nástroje pro Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="01220-140">Get the Entity Framework Core tools</span></span>

<span data-ttu-id="01220-141">Můžete nainstalovat nástroje pro provádění úloh souvisejících s EF Core v projektu, jako je vytvoření a použití migrace databáze nebo vytvoření modelu EF Core na základě existující databáze.</span><span class="sxs-lookup"><span data-stu-id="01220-141">You can install tools to carry out EF Core-related tasks in your project, like creating and applying database migrations, or creating an EF Core model based on an existing database.</span></span>

<span data-ttu-id="01220-142">K dispozici jsou dvě sady nástrojů:</span><span class="sxs-lookup"><span data-stu-id="01220-142">Two sets of tools are available:</span></span>

* <span data-ttu-id="01220-143">[Nástroje rozhraní příkazového řádku .NET Core (CLI)](xref:core/miscellaneous/cli/dotnet) se dají použít v systému Windows, Linux nebo MacOS.</span><span class="sxs-lookup"><span data-stu-id="01220-143">The [.NET Core command-line interface (CLI) tools](xref:core/miscellaneous/cli/dotnet) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="01220-144">Tyto příkazy začínají na `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="01220-144">These commands begin with `dotnet ef`.</span></span>

* <span data-ttu-id="01220-145">[Nástroje konzoly Správce balíčků (PMC)](xref:core/miscellaneous/cli/powershell) se spouštějí v aplikaci Visual Studio ve Windows.</span><span class="sxs-lookup"><span data-stu-id="01220-145">The [Package Manager Console (PMC) tools](xref:core/miscellaneous/cli/powershell) run in Visual Studio on Windows.</span></span> <span data-ttu-id="01220-146">Tyto příkazy začínají příkazem, například `Add-Migration``Update-Database`.</span><span class="sxs-lookup"><span data-stu-id="01220-146">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="01220-147">I když můžete použít `dotnet ef` příkazy z konzoly Správce balíčků, doporučuje se použít nástroje konzoly Správce balíčků při použití sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="01220-147">Although you can also use the `dotnet ef` commands from the Package Manager Console, it's recommended to use the Package Manager Console tools when you're using Visual Studio:</span></span>

* <span data-ttu-id="01220-148">Automaticky pracují s aktuálním projektem vybraným v PMC v aplikaci Visual Studio, aniž by museli ručně přepínat adresáře.</span><span class="sxs-lookup"><span data-stu-id="01220-148">They automatically work with the current project selected in the PMC in Visual Studio, without requiring manually switching directories.</span></span>  

* <span data-ttu-id="01220-149">Po dokončení příkazu automaticky otevřou soubory vygenerované příkazy v aplikaci Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01220-149">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a><span data-ttu-id="01220-150">Získat nástroje pro .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="01220-150">Get the .NET Core CLI tools</span></span>

<span data-ttu-id="01220-151">.NET Core CLI nástroje vyžadují .NET Core SDK, zmíněné dříve v části [požadavky](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="01220-151">.NET Core CLI tools require the .NET Core SDK, mentioned earlier in [Prerequisites](#prerequisites).</span></span>

<span data-ttu-id="01220-152">Příkazy `dotnet ef` jsou součástí aktuálních verzí .NET Core SDK, ale pokud chcete povolit příkazy v konkrétním projektu, je nutné nainstalovat balíček `Microsoft.EntityFrameworkCore.Design`:</span><span class="sxs-lookup"><span data-stu-id="01220-152">The `dotnet ef` commands are included in current versions of the .NET Core SDK, but to enable the commands on a specific project, you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]
> <span data-ttu-id="01220-153">Vždy používejte verzi balíčku nástroje, která odpovídá hlavní verzi běhových balíčků.</span><span class="sxs-lookup"><span data-stu-id="01220-153">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="01220-154">Získat nástroje konzoly Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="01220-154">Get the Package Manager Console tools</span></span>

<span data-ttu-id="01220-155">Chcete-li získat nástroje konzoly Správce balíčků pro EF Core, nainstalujte balíček `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="01220-155">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="01220-156">Například ze sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="01220-156">For example, from Visual Studio:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="01220-157">Pro ASP.NET Core aplikace je tento balíček zahrnutý automaticky.</span><span class="sxs-lookup"><span data-stu-id="01220-157">For ASP.NET Core apps, this package is included automatically.</span></span>

## <a name="upgrading-to-the-latest-ef-core"></a><span data-ttu-id="01220-158">Upgrade na nejnovější EF Core</span><span class="sxs-lookup"><span data-stu-id="01220-158">Upgrading to the latest EF Core</span></span>

* <span data-ttu-id="01220-159">Kdykoli vydáte novou verzi EF Core, vydáváme také novou verzi zprostředkovatelů, které jsou součástí projektu EF Core, jako je Microsoft. EntityFrameworkCore. SqlServer, Microsoft. EntityFrameworkCore. sqlite a Microsoft. EntityFrameworkCore. inMemory.</span><span class="sxs-lookup"><span data-stu-id="01220-159">Any time we release a new version of EF Core, we also release a new version of the providers that are part of the EF Core project, like Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite, and Microsoft.EntityFrameworkCore.InMemory.</span></span> <span data-ttu-id="01220-160">Můžete pouze upgradovat na novou verzi zprostředkovatele, abyste získali všechna vylepšení.</span><span class="sxs-lookup"><span data-stu-id="01220-160">You can just upgrade to the new version of the provider to get all the improvements.</span></span>

* <span data-ttu-id="01220-161">EF Core společně s SQL Server a poskytovateli v paměti jsou zahrnuti v aktuálních verzích ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="01220-161">EF Core, together with the SQL Server and the in-memory providers are included in current versions of ASP.NET Core.</span></span> <span data-ttu-id="01220-162">Chcete-li upgradovat existující aplikaci ASP.NET Core na novější verzi EF Core, vždy upgradujte verzi ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="01220-162">To upgrade an existing ASP.NET Core application to a newer version of EF Core, always upgrade the version of ASP.NET Core.</span></span>

* <span data-ttu-id="01220-163">Pokud potřebujete aktualizovat aplikaci, která používá poskytovatele databáze třetí strany, vždy vyhledejte aktualizaci zprostředkovatele, která je kompatibilní s verzí EF Core, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="01220-163">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="01220-164">Například poskytovatelé databáze pro předchozí verze nejsou kompatibilní s verzí 2,0 EF Core Runtime.</span><span class="sxs-lookup"><span data-stu-id="01220-164">For example, database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>

* <span data-ttu-id="01220-165">Poskytovatelé třetích stran pro EF Core obvykle nevydávají verze oprav společně s modulem EF Core Runtime.</span><span class="sxs-lookup"><span data-stu-id="01220-165">Third-party providers for EF Core usually don't release patch versions alongside the EF Core runtime.</span></span> <span data-ttu-id="01220-166">Chcete-li upgradovat aplikaci, která používá poskytovatele třetí strany, na verzi opravy EF Core, může být nutné přidat přímý odkaz na jednotlivé součásti modulu runtime aplikace EF Core, jako je například Microsoft. EntityFrameworkCore a Microsoft. EntityFrameworkCore. relační.</span><span class="sxs-lookup"><span data-stu-id="01220-166">To upgrade an application that uses a third-party provider to a patch version of EF Core, you may need to add a direct reference to individual EF Core runtime components, such as Microsoft.EntityFrameworkCore, and Microsoft.EntityFrameworkCore.Relational.</span></span>

* <span data-ttu-id="01220-167">Pokud upgradujete existující aplikaci na nejnovější verzi EF Core, může být nutné ručně odebrat některé odkazy na starší EF Core balíčky:</span><span class="sxs-lookup"><span data-stu-id="01220-167">If you're upgrading an existing application to the latest version of EF Core, some references to older EF Core packages may need to be removed manually:</span></span>

  * <span data-ttu-id="01220-168">Balíčky pro dobu návrhu, jako je například `Microsoft.EntityFrameworkCore.SqlServer.Design`, již nejsou vyžadovány nebo podporovány EF Core 2,0 a novějším, ale nejsou automaticky odebrány při upgradu ostatních balíčků.</span><span class="sxs-lookup"><span data-stu-id="01220-168">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported from EF Core 2.0 and later, but aren't automatically removed when upgrading the other packages.</span></span>

  * <span data-ttu-id="01220-169">Nástroje rozhraní příkazového řádku .NET jsou součástí sady .NET SDK od verze 2,1, takže odkaz na tento balíček lze odebrat ze souboru projektu:</span><span class="sxs-lookup"><span data-stu-id="01220-169">The .NET CLI tools are included in the .NET SDK since version 2.1, so the reference to that package can be removed from the project file:</span></span>

    ``` xml
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```
