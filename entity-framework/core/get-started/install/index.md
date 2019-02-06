---
title: Instalace Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 5ebc4edba07063ad5e77154adcde5f2664c0d748
ms.sourcegitcommit: 85d17524d8e022f933cde7fc848313f57dfd3eb8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2019
ms.locfileid: "55760519"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="e2132-102">Instalace Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e2132-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e2132-103">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e2132-103">Prerequisites</span></span>

* <span data-ttu-id="e2132-104">Je EF Core [.NET Standard 2.0](/dotnet/standard/net-standard) knihovny.</span><span class="sxs-lookup"><span data-stu-id="e2132-104">EF Core is a [.NET Standard 2.0](/dotnet/standard/net-standard) library.</span></span> <span data-ttu-id="e2132-105">EF Core vyžaduje implementaci rozhraní .NET, která podporuje .NET Standard 2.0 ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="e2132-105">So EF Core requires a .NET implementation that supports .NET Standard 2.0 to run.</span></span> <span data-ttu-id="e2132-106">EF Core můžete odkazovat také jiné knihovny .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="e2132-106">EF Core can also be referenced by other .NET Standard 2.0 libraries.</span></span> 

* <span data-ttu-id="e2132-107">Například můžete použít EF Core můžete vyvíjet aplikace, které cílí na .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2132-107">For example, you can use EF Core to develop apps that target .NET Core.</span></span> <span data-ttu-id="e2132-108">Vytvoření aplikace .NET Core vyžaduje [.NET Core SDK](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="e2132-108">Building .NET Core apps requires the [.NET Core SDK](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="e2132-109">Volitelně můžete také použít vývojové prostředí, jako je Visual Studio, Visual Studio pro Mac nebo Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e2132-109">Optionally, you can also use a development environment like Visual Studio, Visual Studio for Mac, or Visual Studio Code.</span></span> <span data-ttu-id="e2132-110">Další informace najdete [Začínáme s .NET Core](/dotnet/core/get-started).</span><span class="sxs-lookup"><span data-stu-id="e2132-110">For more information, check [Getting Started with .NET Core](/dotnet/core/get-started).</span></span>

* <span data-ttu-id="e2132-111">EF Core můžete použít k vývoji aplikací využívajících rozhraní .NET Framework 4.6.1 nebo později ve Windows pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e2132-111">You can use EF Core to develop applications that target .NET Framework 4.6.1 or later on Windows, using Visual Studio.</span></span> <span data-ttu-id="e2132-112">Doporučuje se nejnovější verzi sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e2132-112">The latest version of Visual Studio is recommended.</span></span> <span data-ttu-id="e2132-113">Pokud chcete použít starší verzi, jako je Visual Studio 2015, ujistěte se, že jste [upgradovat klienta NuGet verzi 3.6.0](https://www.nuget.org/downloads) pro práci s knihovnami .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="e2132-113">If you want to use an older version, like Visual Studio 2015, make sure you [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads) to work with .NET Standard 2.0 libraries.</span></span>

* <span data-ttu-id="e2132-114">EF Core můžete spustit v jiných implementacích rozhraní .NET, jako třeba Xamarin nebo .NET Native.</span><span class="sxs-lookup"><span data-stu-id="e2132-114">EF Core can run on other .NET implementations like Xamarin and .NET Native.</span></span> <span data-ttu-id="e2132-115">Ale v praxi tyto implementace omezení modulu runtime, které mohou ovlivnit, jak dobře EF Core funguje ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e2132-115">But in practice those implementations have runtime limitations that may affect how well EF Core works on your app.</span></span> <span data-ttu-id="e2132-116">Další informace najdete v tématu [implementacím rozhraní .NET, které EF Core](xref:core/platforms/index).</span><span class="sxs-lookup"><span data-stu-id="e2132-116">For more information, see [.NET implementations supported by EF Core](xref:core/platforms/index).</span></span>

* <span data-ttu-id="e2132-117">Nakonec poskytovatelé různých databází může vyžadovat konkrétní databázi verzí vyhledávacích strojů, implementace .NET nebo operační systémy.</span><span class="sxs-lookup"><span data-stu-id="e2132-117">Finally, different database providers may require specific database engine versions, .NET implementations, or operating systems.</span></span> <span data-ttu-id="e2132-118">Ujistěte se, že [poskytovatele databáze EF Core](xref:core/providers/index) je k dispozici, který podporuje správné prostředí pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e2132-118">Make sure an [EF Core database provider](xref:core/providers/index) is available that supports the right environment for your application.</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="e2132-119">Získat modul runtime Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e2132-119">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="e2132-120">EF Core přidat do aplikace, nainstalujte balíček NuGet pro poskytovatele databáze, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="e2132-120">To add EF Core to an application, install the NuGet package for the database provider you want to use.</span></span>

<span data-ttu-id="e2132-121">Pokud vytváříte aplikaci ASP.NET Core, není nutné instalovat v paměti a poskytovatelé serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e2132-121">If you're building an ASP.NET Core application, you don't need to install the in-memory and SQL Server providers.</span></span> <span data-ttu-id="e2132-122">Tyto poskytovatelé jsou součástí aktuálních verzí ASP.NET Core, společně s runtime EF Core.</span><span class="sxs-lookup"><span data-stu-id="e2132-122">Those providers are included in current versions of ASP.NET Core, alongside the EF Core runtime.</span></span>  

<span data-ttu-id="e2132-123">Pokud chcete nainstalovat nebo aktualizovat balíčky NuGet, můžete použít .NET Core rozhraní příkazového řádku (CLI), dialogové okno Správce balíčků Visual Studio nebo Konzola správce balíčků Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e2132-123">To install or update NuGet packages, you can use the .NET Core command-line interface (CLI), the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

### <a name="net-core-cli"></a><span data-ttu-id="e2132-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e2132-124">.NET Core CLI</span></span>

* <span data-ttu-id="e2132-125">Použijte následující příkaz rozhraní příkazového řádku .NET Core z příkazového řádku operačního systému pro instalaci nebo aktualizaci zprostředkovatele EF Core SQL Server:</span><span class="sxs-lookup"><span data-stu-id="e2132-125">Use the following .NET Core CLI command from the operating system's command line to install or update the EF Core SQL Server provider:</span></span>

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* <span data-ttu-id="e2132-126">Můžete určit konkrétní verzi `dotnet add package` příkaz a `-v` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="e2132-126">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="e2132-127">Například chcete-li instalovat balíčky EF Core 2.2.0, přidejte `-v 2.2.0` k příkazu.</span><span class="sxs-lookup"><span data-stu-id="e2132-127">For example, to install EF Core 2.2.0 packages, append `-v 2.2.0` to the command.</span></span>

<span data-ttu-id="e2132-128">Další informace najdete v tématu [nástroje rozhraní příkazového řádku (CLI) .NET](/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="e2132-128">For more information, see [.NET command-line interface (CLI) tools](/dotnet/core/tools/).</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="e2132-129">Dialogové okno Správce balíčků NuGet sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2132-129">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="e2132-130">V nabídce sady Visual Studio vyberte **Projekt > spravovat balíčky NuGet**</span><span class="sxs-lookup"><span data-stu-id="e2132-130">From the Visual Studio menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="e2132-131">Klikněte na **Procházet** nebo **aktualizace** kartu</span><span class="sxs-lookup"><span data-stu-id="e2132-131">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="e2132-132">Chcete-li nainstalovat nebo aktualizovat zprostředkovatele SQL Server, vyberte `Microsoft.EntityFrameworkCore.SqlServer` balení a potvrďte.</span><span class="sxs-lookup"><span data-stu-id="e2132-132">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="e2132-133">Další informace najdete v tématu [dialogové okno Správce balíčků NuGet](/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="e2132-133">For more information, see [NuGet Package Manager Dialog](/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="e2132-134">Konzola správce balíčků NuGet sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2132-134">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="e2132-135">V nabídce sady Visual Studio vyberte **nástroje > Správce balíčků NuGet > Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="e2132-135">From the Visual Studio menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="e2132-136">Pokud chcete nainstalovat zprostředkovatele SQL Server, spusťte následující příkaz v konzole Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="e2132-136">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="e2132-137">Chcete-li aktualizovat poskytovatele, použijte `Update-Package` příkazu.</span><span class="sxs-lookup"><span data-stu-id="e2132-137">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="e2132-138">Chcete-li zadat konkrétní verzi, použijte `-Version` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="e2132-138">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="e2132-139">Například chcete-li instalovat balíčky EF Core 2.2.0, přidejte `-Version 2.2.0` k příkazům</span><span class="sxs-lookup"><span data-stu-id="e2132-139">For example, to install EF Core 2.2.0 packages, append `-Version 2.2.0` to the commands</span></span>

<span data-ttu-id="e2132-140">Další informace najdete v tématu [Konzola správce balíčků](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="e2132-140">For more information, see [Package Manager Console](/nuget/tools/package-manager-console).</span></span>

## <a name="get-the-entity-framework-core-tools"></a><span data-ttu-id="e2132-141">Získat nástroje Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e2132-141">Get the Entity Framework Core tools</span></span>

<span data-ttu-id="e2132-142">Můžete nainstalovat nástroje provádět úlohy související s EF Core ve vašem projektu, jako jsou vytvoření a použití migrace databází nebo vytvoření modelu EF Core založené na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="e2132-142">You can install tools to carry out EF Core-related tasks in your project, like creating and applying database migrations, or creating an EF Core model based on an existing database.</span></span>

<span data-ttu-id="e2132-143">K dispozici jsou dvě sady nástrojů:</span><span class="sxs-lookup"><span data-stu-id="e2132-143">Two sets of tools are available:</span></span>

* <span data-ttu-id="e2132-144">[Nástroje rozhraní příkazového řádku (CLI) pro .NET Core](xref:core/miscellaneous/cli/dotnet) lze použít na Windows, Linux nebo macOS.</span><span class="sxs-lookup"><span data-stu-id="e2132-144">The [.NET Core command-line interface (CLI) tools](xref:core/miscellaneous/cli/dotnet) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="e2132-145">Tyto příkazy začínají `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="e2132-145">These commands begin with `dotnet ef`.</span></span> 

* <span data-ttu-id="e2132-146">[Nástroje konzoly Správce balíčků (PMC)](xref:core/miscellaneous/cli/powershell) spustit v sadě Visual Studio na Windows.</span><span class="sxs-lookup"><span data-stu-id="e2132-146">The [Package Manager Console (PMC) tools](xref:core/miscellaneous/cli/powershell) run in Visual Studio on Windows.</span></span> <span data-ttu-id="e2132-147">Tyto příkazy spustit s operací, například `Add-Migration`, `Update-Database`.</span><span class="sxs-lookup"><span data-stu-id="e2132-147">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="e2132-148">I když můžete použít také `dotnet ef` příkazy z konzoly Správce balíčků, doporučuje se používat nástroje konzoly Správce balíčků při používání sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="e2132-148">Although you can also use the `dotnet ef` commands from the Package Manager Console, it's recommended to use the Package Manager Console tools when you're using Visual Studio:</span></span>

* <span data-ttu-id="e2132-149">Automaticky fungují s aktuální projekt vybraný v konzole PMC v sadě Visual Studio bez nutnosti ručně přepnout adresáře.</span><span class="sxs-lookup"><span data-stu-id="e2132-149">They automatically work with the current project selected in the PMC in Visual Studio, without requiring manually switching directories.</span></span>  

* <span data-ttu-id="e2132-150">Automaticky otevřou soubory generované záznamem pro příkazy v sadě Visual Studio po dokončení příkazu.</span><span class="sxs-lookup"><span data-stu-id="e2132-150">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a><span data-ttu-id="e2132-151">Získání nástrojů rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e2132-151">Get the .NET Core CLI tools</span></span>

<span data-ttu-id="e2132-152">Nástroje rozhraní příkazového řádku .NET core vyžaduje .NET Core SDK, uvedené výše v [požadavky](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="e2132-152">.NET Core CLI tools require the .NET Core SDK, mentioned earlier in [Prerequisites](#prerequisites).</span></span>

<span data-ttu-id="e2132-153">`dotnet ef` Příkazy jsou zahrnuté v aktuálních verzích .NET Core SDK, ale aby příkazy v konkrétním projektu, je třeba nainstalovat `Microsoft.EntityFrameworkCore.Design` balíčku:</span><span class="sxs-lookup"><span data-stu-id="e2132-153">The `dotnet ef` commands are included in current versions of the .NET Core SDK, but to enable the commands on a specific project, you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="e2132-154">Pro aplikace ASP.NET Core pro tento balíček je přiložena automaticky.</span><span class="sxs-lookup"><span data-stu-id="e2132-154">For ASP.NET Core apps, this package is included automatically.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="e2132-155">Vždy používejte verzi sady nástrojů, který odpovídá hlavní verzi balíčky runtime.</span><span class="sxs-lookup"><span data-stu-id="e2132-155">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="e2132-156">Získat nástroje konzoly Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="e2132-156">Get the Package Manager Console tools</span></span>

<span data-ttu-id="e2132-157">Chcete-li získat Konzola správce balíčků nástroje na EF Core, nainstalovat `Microsoft.EntityFrameworkCore.Tools` balíčku.</span><span class="sxs-lookup"><span data-stu-id="e2132-157">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="e2132-158">Například ze sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="e2132-158">For example, from Visual Studio:</span></span>

``` PowerShell  
Install-Package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="e2132-159">Pro aplikace ASP.NET Core pro tento balíček je přiložena automaticky.</span><span class="sxs-lookup"><span data-stu-id="e2132-159">For ASP.NET Core apps, this package is included automatically.</span></span>

## <a name="upgrading-to-the-latest-ef-core"></a><span data-ttu-id="e2132-160">Upgrade na nejnovější EF Core</span><span class="sxs-lookup"><span data-stu-id="e2132-160">Upgrading to the latest EF Core</span></span>

* <span data-ttu-id="e2132-161">Kdykoli můžeme vydat novou verzi EF Core, také uvolňujeme nové verze zprostředkovatele, které jsou součástí projektu EF Core, jako je Microsoft.EntityFrameworkCore.SqlServer Microsoft.EntityFrameworkCore.Sqlite, a Microsoft.EntityFrameworkCore.InMemory.</span><span class="sxs-lookup"><span data-stu-id="e2132-161">Any time we release a new version of EF Core, we also release a new version of the providers that are part of the EF Core project, like Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite, and Microsoft.EntityFrameworkCore.InMemory.</span></span> <span data-ttu-id="e2132-162">Pouze můžete upgradovat na novou verzi poskytovatele za účelem získání všech vylepšení.</span><span class="sxs-lookup"><span data-stu-id="e2132-162">You can just upgrade to the new version of the provider to get all the improvements.</span></span> 

* <span data-ttu-id="e2132-163">EF Core, spolu se službou SQL Server a poskytovatele v paměti jsou zahrnuté v aktuálních verzích ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2132-163">EF Core, together with the SQL Server and the in-memory providers are included in current versions of ASP.NET Core.</span></span> <span data-ttu-id="e2132-164">Upgrade stávající aplikace ASP.NET Core na novější verzi EF Core, vždy upgradujte na verzi ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2132-164">To upgrade an existing ASP.NET Core application to a newer version of EF Core, always upgrade the version of ASP.NET Core.</span></span>

* <span data-ttu-id="e2132-165">Pokud je potřeba aktualizovat aplikaci, která používá databázi jiného poskytovatele, vždy vyhledejte aktualizace zprostředkovatele, který je kompatibilní s verzí EF Core, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="e2132-165">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="e2132-166">Poskytovatelé databází pro předchozí verze například nejsou kompatibilní s verzí modulu runtime EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="e2132-166">For example, database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>

* <span data-ttu-id="e2132-167">Zprostředkovatelé třetí strany pro jádro EF Core obvykle není konečné verze opravy společně s runtime EF Core.</span><span class="sxs-lookup"><span data-stu-id="e2132-167">Third-party providers for EF Core usually don't release patch versions alongside the EF Core runtime.</span></span> <span data-ttu-id="e2132-168">Pokud chcete upgradovat aplikaci, která používá poskytovatele třetí strany na verzi opravy EF Core, budete muset přidat přímý odkaz na jednotlivé součásti modulu runtime EF Core, jako je například Microsoft.EntityFrameworkCore a Microsoft.EntityFrameworkCore.Relational.</span><span class="sxs-lookup"><span data-stu-id="e2132-168">To upgrade an application that uses a third-party provider to a patch version of EF Core, you may need to add a direct reference to individual EF Core runtime components, such as Microsoft.EntityFrameworkCore, and Microsoft.EntityFrameworkCore.Relational.</span></span>

* <span data-ttu-id="e2132-169">Pokud upgradujete existující aplikaci na nejnovější verzi EF Core, některé odkazy na balíčky starší EF Core může být potřeba odebrat ručně:</span><span class="sxs-lookup"><span data-stu-id="e2132-169">If you're upgrading an existing application to the latest version of EF Core, some references to older EF Core packages may need to be removed manually:</span></span>

  * <span data-ttu-id="e2132-170">Databáze jako poskytovatel návrhu balíčky `Microsoft.EntityFrameworkCore.SqlServer.Design` už nejsou požadované nebo podporovaných v EF Core 2.0 a vyšší, ale neodeberou se automaticky při upgradu balíčky.</span><span class="sxs-lookup"><span data-stu-id="e2132-170">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported from EF Core 2.0 and later, but aren't automatically removed when upgrading the other packages.</span></span>

  * <span data-ttu-id="e2132-171">Nástroje rozhraní příkazového řádku .NET jsou zahrnuty v sadě .NET SDK od verze 2.1, tak odkaz na tento balíček je odebrat ze souboru projektu:</span><span class="sxs-lookup"><span data-stu-id="e2132-171">The .NET CLI tools are included in the .NET SDK since version 2.1, so the reference to that package can be removed from the project file:</span></span>

    ```
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```

* <span data-ttu-id="e2132-172">Aplikací určených pro rozhraní .NET Framework možná bude nutné změny pro práci s knihovnami .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="e2132-172">Applications that target .NET Framework may need changes to work with .NET Standard 2.0 libraries:</span></span>

  * <span data-ttu-id="e2132-173">Upravte soubor projektu a ujistěte se, že ve skupině počáteční vlastnosti se zobrazí následující položky:</span><span class="sxs-lookup"><span data-stu-id="e2132-173">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * <span data-ttu-id="e2132-174">Projekty testů ujistěte, že je k dispozici následující položky:</span><span class="sxs-lookup"><span data-stu-id="e2132-174">For test projects, also make sure the following entry is present:</span></span>

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
