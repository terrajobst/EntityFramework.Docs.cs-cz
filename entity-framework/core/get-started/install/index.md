---
title: Instalace jádra entity – jádro EF
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 6575b1ac028f8b67b49ca7f4e49d6f19500be98f
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136175"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="c5aa8-102">Instalace jádra entity frameworku</span><span class="sxs-lookup"><span data-stu-id="c5aa8-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c5aa8-103">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c5aa8-103">Prerequisites</span></span>

* <span data-ttu-id="c5aa8-104">EF Core je knihovna [.NET Standard 2.0.](/dotnet/standard/net-standard)</span><span class="sxs-lookup"><span data-stu-id="c5aa8-104">EF Core is a [.NET Standard 2.0](/dotnet/standard/net-standard) library.</span></span> <span data-ttu-id="c5aa8-105">Takže EF Core vyžaduje implementaci .NET, která podporuje .NET Standard 2.0 ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-105">So EF Core requires a .NET implementation that supports .NET Standard 2.0 to run.</span></span> <span data-ttu-id="c5aa8-106">Ef Core lze také odkazovat na jiné knihovny .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-106">EF Core can also be referenced by other .NET Standard 2.0 libraries.</span></span>

* <span data-ttu-id="c5aa8-107">Například můžete použít EF Core k vývoji aplikací, které cílí na .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-107">For example, you can use EF Core to develop apps that target .NET Core.</span></span> <span data-ttu-id="c5aa8-108">Vytváření aplikací .NET Core vyžaduje [.NET Core SDK](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="c5aa8-108">Building .NET Core apps requires the [.NET Core SDK](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="c5aa8-109">Volitelně můžete také použít vývojové prostředí, jako je [Visual Studio](https://visualstudio.microsoft.com/vs), Visual Studio [pro Mac](https://visualstudio.microsoft.com/vs/mac)nebo Visual [Studio Code](https://code.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="c5aa8-109">Optionally, you can also use a development environment like [Visual Studio](https://visualstudio.microsoft.com/vs), [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac), or [Visual Studio Code](https://code.visualstudio.com).</span></span> <span data-ttu-id="c5aa8-110">Další informace naleznete v souboru [Začínáme s rozhraním .NET Core](/dotnet/core/get-started).</span><span class="sxs-lookup"><span data-stu-id="c5aa8-110">For more information, check [Getting Started with .NET Core](/dotnet/core/get-started).</span></span>

* <span data-ttu-id="c5aa8-111">EF Core můžete použít k vývoji aplikací v systému Windows pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-111">You can use EF Core to develop applications on Windows using Visual Studio.</span></span> <span data-ttu-id="c5aa8-112">Doporučujeme nejnovější verzi [sady Visual Studio.](https://visualstudio.microsoft.com/vs)</span><span class="sxs-lookup"><span data-stu-id="c5aa8-112">The latest version of [Visual Studio](https://visualstudio.microsoft.com/vs) is recommended.</span></span>

* <span data-ttu-id="c5aa8-113">EF Core lze spustit na jiných implementacích .NET, jako je [Xamarin](https://dotnet.microsoft.com/apps/xamarin) a .NET Native.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-113">EF Core can run on other .NET implementations like [Xamarin](https://dotnet.microsoft.com/apps/xamarin) and .NET Native.</span></span> <span data-ttu-id="c5aa8-114">Ale v praxi tyto implementace mají omezení za běhu, které mohou ovlivnit, jak dobře EF Core funguje na vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-114">But in practice those implementations have runtime limitations that may affect how well EF Core works on your app.</span></span> <span data-ttu-id="c5aa8-115">Další informace naleznete [v tématu implementace rozhraní .NET podporované EF Core](xref:core/platforms/index).</span><span class="sxs-lookup"><span data-stu-id="c5aa8-115">For more information, see [.NET implementations supported by EF Core](xref:core/platforms/index).</span></span>

* <span data-ttu-id="c5aa8-116">Nakonec mohou různí poskytovatelé databází vyžadovat konkrétní verze databázového stroje, implementace rozhraní .NET nebo operační systémy.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-116">Finally, different database providers may require specific database engine versions, .NET implementations, or operating systems.</span></span> <span data-ttu-id="c5aa8-117">Ujistěte se, že je k dispozici [zprostředkovatel databáze EF Core,](xref:core/providers/index) který podporuje správné prostředí pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-117">Make sure an [EF Core database provider](xref:core/providers/index) is available that supports the right environment for your application.</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="c5aa8-118">Získání runtime jádra entity frameworku</span><span class="sxs-lookup"><span data-stu-id="c5aa8-118">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="c5aa8-119">Chcete-li přidat EF Core do aplikace, nainstalujte balíček NuGet pro poskytovatele databáze, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-119">To add EF Core to an application, install the NuGet package for the database provider you want to use.</span></span>

<span data-ttu-id="c5aa8-120">Pokud vytváříte ASP.NET základní aplikace, není nutné instalovat v paměti a SQL Server zprostředkovatelů.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-120">If you're building an ASP.NET Core application, you don't need to install the in-memory and SQL Server providers.</span></span> <span data-ttu-id="c5aa8-121">Tito poskytovatelé jsou zahrnuti v aktuálních verzích ASP.NET Core, spolu s runtime EF Core.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-121">Those providers are included in current versions of ASP.NET Core, alongside the EF Core runtime.</span></span>  

<span data-ttu-id="c5aa8-122">Chcete-li nainstalovat nebo aktualizovat balíčky NuGet, můžete použít rozhraní příkazového řádku .NET Core (CLI), dialogové okno Správce balíčků sady Visual Studio nebo konzolu Správce balíčků sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-122">To install or update NuGet packages, you can use the .NET Core command-line interface (CLI), the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

### <a name="net-core-cli"></a><span data-ttu-id="c5aa8-123">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="c5aa8-123">.NET Core CLI</span></span>

* <span data-ttu-id="c5aa8-124">K instalaci nebo aktualizaci zprostředkovatele serveru SQL Server EF Core použijte následující příkaz příkazu .NET Core CLI z příkazového řádku operačního systému:</span><span class="sxs-lookup"><span data-stu-id="c5aa8-124">Use the following .NET Core CLI command from the operating system's command line to install or update the EF Core SQL Server provider:</span></span>

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* <span data-ttu-id="c5aa8-125">Pomocí `-v` modifikátoru `dotnet add package` můžete v příkazu určit určitou verzi.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-125">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="c5aa8-126">Chcete-li například nainstalovat balíčky EF Core `-v 2.2.0` 2.2.0, připojte příkaz.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-126">For example, to install EF Core 2.2.0 packages, append `-v 2.2.0` to the command.</span></span>

<span data-ttu-id="c5aa8-127">For more information, see [.NET command-line interface (CLI) tools](/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="c5aa8-127">For more information, see [.NET command-line interface (CLI) tools](/dotnet/core/tools/).</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="c5aa8-128">Dialogové okno Správce balíčků aplikace Visual Studio NuGet</span><span class="sxs-lookup"><span data-stu-id="c5aa8-128">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="c5aa8-129">V nabídce Visual Studio vyberte **Project > Manage NuGet Packages .**</span><span class="sxs-lookup"><span data-stu-id="c5aa8-129">From the Visual Studio menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="c5aa8-130">Klikněte na kartu **Procházet** nebo **Aktualizace.**</span><span class="sxs-lookup"><span data-stu-id="c5aa8-130">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="c5aa8-131">Chcete-li nainstalovat nebo aktualizovat zprostředkovatele serveru SQL Server, vyberte `Microsoft.EntityFrameworkCore.SqlServer` balíček a potvrďte.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-131">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="c5aa8-132">Další informace naleznete [v tématu NuGet Package Manager Dialog](/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="c5aa8-132">For more information, see [NuGet Package Manager Dialog](/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="c5aa8-133">Konzola Správce balíčků aplikace Visual Studio NuGet</span><span class="sxs-lookup"><span data-stu-id="c5aa8-133">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="c5aa8-134">V nabídce Visual Studio vyberte **Nástroje > Správce balíčků NuGet > konzoli správce balíčků.**</span><span class="sxs-lookup"><span data-stu-id="c5aa8-134">From the Visual Studio menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="c5aa8-135">Chcete-li nainstalovat zprostředkovatele serveru SQL Server, spusťte v konzole Správce balíčků následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c5aa8-135">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```

* <span data-ttu-id="c5aa8-136">Chcete-li aktualizovat zprostředkovatele, použijte `Update-Package` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-136">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="c5aa8-137">Chcete-li určit konkrétní `-Version` verzi, použijte modifikátor.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-137">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="c5aa8-138">Chcete-li například nainstalovat balíčky EF Core `-Version 2.2.0` 2.2.0, připojujte k příkazům</span><span class="sxs-lookup"><span data-stu-id="c5aa8-138">For example, to install EF Core 2.2.0 packages, append `-Version 2.2.0` to the commands</span></span>

<span data-ttu-id="c5aa8-139">Další informace naleznete v [tématu Package Manager Console](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="c5aa8-139">For more information, see [Package Manager Console](/nuget/tools/package-manager-console).</span></span>

## <a name="get-the-entity-framework-core-tools"></a><span data-ttu-id="c5aa8-140">Získejte základní nástroje frameworku entit</span><span class="sxs-lookup"><span data-stu-id="c5aa8-140">Get the Entity Framework Core tools</span></span>

<span data-ttu-id="c5aa8-141">Můžete nainstalovat nástroje pro provádění úloh souvisejících s EF Core v projektu, jako je vytváření a použití migrace databáze nebo vytvoření modelu EF Core na základě existující databáze.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-141">You can install tools to carry out EF Core-related tasks in your project, like creating and applying database migrations, or creating an EF Core model based on an existing database.</span></span>

<span data-ttu-id="c5aa8-142">K dispozici jsou dvě sady nástrojů:</span><span class="sxs-lookup"><span data-stu-id="c5aa8-142">Two sets of tools are available:</span></span>

* <span data-ttu-id="c5aa8-143">Nástroje [rozhraní příkazového řádku .NET Core (CLI)](xref:core/miscellaneous/cli/dotnet) lze použít ve Windows, Linuxu nebo macOS.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-143">The [.NET Core command-line interface (CLI) tools](xref:core/miscellaneous/cli/dotnet) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="c5aa8-144">Tyto příkazy `dotnet ef`začínají na .</span><span class="sxs-lookup"><span data-stu-id="c5aa8-144">These commands begin with `dotnet ef`.</span></span>

* <span data-ttu-id="c5aa8-145">[Nástroje konzoly Správce balíčků (PMC)](xref:core/miscellaneous/cli/powershell) jsou spuštěny v sadě Visual Studio v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-145">The [Package Manager Console (PMC) tools](xref:core/miscellaneous/cli/powershell) run in Visual Studio on Windows.</span></span> <span data-ttu-id="c5aa8-146">Tyto příkazy začínají slovesem, například `Add-Migration`. `Update-Database`</span><span class="sxs-lookup"><span data-stu-id="c5aa8-146">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="c5aa8-147">I když můžete `dotnet ef` příkazy použít také z konzoly Správce balíčků, doporučujeme používat nástroje konzoly Správce balíčků, když používáte Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c5aa8-147">Although you can also use the `dotnet ef` commands from the Package Manager Console, it's recommended to use the Package Manager Console tools when you're using Visual Studio:</span></span>

* <span data-ttu-id="c5aa8-148">Automaticky pracují s aktuálním projektem vybraným v PMC v sadě Visual Studio, aniž by bylo nutné ručně přepínat adresáře.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-148">They automatically work with the current project selected in the PMC in Visual Studio, without requiring manually switching directories.</span></span>  

* <span data-ttu-id="c5aa8-149">Automaticky otevírají soubory generované příkazy v sadě Visual Studio po dokončení příkazu.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-149">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a><span data-ttu-id="c5aa8-150">Získání nástrojů rozhraní .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c5aa8-150">Get the .NET Core CLI tools</span></span>

<span data-ttu-id="c5aa8-151">Nástroje rozhraní CLI .NET Core vyžadují sadku .NET Core SDK, která je uvedena dříve v [části Požadavky](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="c5aa8-151">.NET Core CLI tools require the .NET Core SDK, mentioned earlier in [Prerequisites](#prerequisites).</span></span>

<span data-ttu-id="c5aa8-152">Příkazy `dotnet ef` jsou zahrnuty v aktuálních verzích sady .NET Core SDK, ale chcete-li `Microsoft.EntityFrameworkCore.Design` povolit příkazy v konkrétním projektu, je nutné nainstalovat balíček:</span><span class="sxs-lookup"><span data-stu-id="c5aa8-152">The `dotnet ef` commands are included in current versions of the .NET Core SDK, but to enable the commands on a specific project, you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]
> <span data-ttu-id="c5aa8-153">Vždy používejte verzi balíčku nástroje, který odpovídá hlavní verzi balíčků runtime.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-153">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="c5aa8-154">Získání nástrojů konzoly Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="c5aa8-154">Get the Package Manager Console tools</span></span>

<span data-ttu-id="c5aa8-155">Chcete-li získat nástroje konzoly Správce `Microsoft.EntityFrameworkCore.Tools` balíčků pro EF Core, nainstalujte balíček.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-155">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="c5aa8-156">Například z Visual Studia:</span><span class="sxs-lookup"><span data-stu-id="c5aa8-156">For example, from Visual Studio:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="c5aa8-157">Pro ASP.NET aplikace Core je tento balíček součástí automaticky.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-157">For ASP.NET Core apps, this package is included automatically.</span></span>

## <a name="upgrading-to-the-latest-ef-core"></a><span data-ttu-id="c5aa8-158">Upgrade na nejnovější ef jádro</span><span class="sxs-lookup"><span data-stu-id="c5aa8-158">Upgrading to the latest EF Core</span></span>

* <span data-ttu-id="c5aa8-159">Pokaždé, když vydáme novou verzi EF Core, vydáme také novou verzi zprostředkovatelů, kteří jsou součástí projektu EF Core, jako jsou Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityCore.Sqlite a Microsoft.EntityFrameworkCore.InMemory.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-159">Any time we release a new version of EF Core, we also release a new version of the providers that are part of the EF Core project, like Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite, and Microsoft.EntityFrameworkCore.InMemory.</span></span> <span data-ttu-id="c5aa8-160">Můžete jen upgradovat na novou verzi poskytovatele získat všechna vylepšení.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-160">You can just upgrade to the new version of the provider to get all the improvements.</span></span>

* <span data-ttu-id="c5aa8-161">EF Core, spolu s SQL Server a zprostředkovatelé v paměti jsou zahrnuty v aktuálních verzích ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-161">EF Core, together with the SQL Server and the in-memory providers are included in current versions of ASP.NET Core.</span></span> <span data-ttu-id="c5aa8-162">Chcete-li upgradovat existující ASP.NET základní aplikace na novější verzi EF Core, vždy upgradujte verzi ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-162">To upgrade an existing ASP.NET Core application to a newer version of EF Core, always upgrade the version of ASP.NET Core.</span></span>

* <span data-ttu-id="c5aa8-163">Pokud potřebujete aktualizovat aplikaci, která používá zprostředkovatele databáze jiného výrobce, vždy zkontrolujte aktualizaci zprostředkovatele, která je kompatibilní s verzí EF Core, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-163">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="c5aa8-164">Například zprostředkovatelé databáze pro předchozí verze nejsou kompatibilní s verzí 2.0 runtime EF Core.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-164">For example, database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>

* <span data-ttu-id="c5aa8-165">Poskytovatelé třetích stran pro EF Core obvykle nevydávají verze oprav vedle runtime EF Core.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-165">Third-party providers for EF Core usually don't release patch versions alongside the EF Core runtime.</span></span> <span data-ttu-id="c5aa8-166">Chcete-li upgradovat aplikaci, která používá poskytovatele třetí strany na verzi opravy EF Core, budete muset přidat přímý odkaz na jednotlivé komponenty ef core runtime, jako jsou Microsoft.EntityFrameworkCore a Microsoft.EntityFrameworkCore.Relational.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-166">To upgrade an application that uses a third-party provider to a patch version of EF Core, you may need to add a direct reference to individual EF Core runtime components, such as Microsoft.EntityFrameworkCore, and Microsoft.EntityFrameworkCore.Relational.</span></span>

* <span data-ttu-id="c5aa8-167">Pokud upgradujete existující aplikaci na nejnovější verzi EF Core, může být nutné ručně odebrat některé odkazy na starší balíčky EF Core:</span><span class="sxs-lookup"><span data-stu-id="c5aa8-167">If you're upgrading an existing application to the latest version of EF Core, some references to older EF Core packages may need to be removed manually:</span></span>

  * <span data-ttu-id="c5aa8-168">Balíčky návrhu zprostředkovatele `Microsoft.EntityFrameworkCore.SqlServer.Design` databáze, jako jsou již nejsou požadovány nebo podporovány z EF Core 2.0 a novější, ale nejsou automaticky odebrány při upgradu ostatní balíčky.</span><span class="sxs-lookup"><span data-stu-id="c5aa8-168">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported from EF Core 2.0 and later, but aren't automatically removed when upgrading the other packages.</span></span>

  * <span data-ttu-id="c5aa8-169">Nástroje rozhraní .NET CLI jsou zahrnuty v sdk .NET od verze 2.1, takže odkaz na tento balíček lze odebrat ze souboru projektu:</span><span class="sxs-lookup"><span data-stu-id="c5aa8-169">The .NET CLI tools are included in the .NET SDK since version 2.1, so the reference to that package can be removed from the project file:</span></span>

    ``` xml
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```
