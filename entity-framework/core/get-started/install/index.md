---
title: Instalace Entiy Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 7831e6a54e885cf0b89ef3eef2cd81a9292df606
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250319"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="af870-102">Instalace Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="af870-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="af870-103">Požadavky</span><span class="sxs-lookup"><span data-stu-id="af870-103">Prerequisites</span></span>

* <span data-ttu-id="af870-104">Pro vývoj aplikací určených pro .NET Core 2.1, nainstalujte [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="af870-104">To develop apps that target .NET Core 2.1, install [the .NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="af870-105">Sada SDK musí být nainstalovaný, i v případě, že máte nejnovější verzi sady Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="af870-105">The SDK has to be installed even if you have the latest version of Visual Studio 2017.</span></span>

* <span data-ttu-id="af870-106">Použití sady Visual Studio pro vývoj aplikací určených pro .NET Core 2.1, nainstalujte Visual Studio 2017 verze 15.7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="af870-106">To use Visual Studio for development of apps that target .NET Core 2.1, install Visual Studio 2017 version 15.7 or later.</span></span>

* <span data-ttu-id="af870-107">Entity Framework 2.1 v aplikacích ASP.NET Core, použití technologie ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="af870-107">To use Entity Framework 2.1 in ASP.NET Core applications, use ASP.NET Core 2.1.</span></span> <span data-ttu-id="af870-108">Aplikace, které používají starší verze technologie ASP.NET Core, musí být aktualizovány na verzi 2.1.</span><span class="sxs-lookup"><span data-stu-id="af870-108">Applications that use earlier versions of ASP.NET Core must be updated to 2.1.</span></span>

* <span data-ttu-id="af870-109">Visual Studio 2015 můžete použít pro aplikace, které cílí .NET Framework 4.6.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="af870-109">You can use Visual Studio 2015 for apps that target the .NET Framework 4.6.1 or later.</span></span> <span data-ttu-id="af870-110">Ale potřebujete verzi balíčku nuget, který si je vědoma .NET Standard 2.0 a jeho rozhraní kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="af870-110">But you need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="af870-111">Chcete-li získat, který v sadě Visual Studio 2015, [upgradovat klienta NuGet verzi 3.6.0](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="af870-111">To get that in Visual Studio 2015, [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads).</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="af870-112">Získat modul runtime Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="af870-112">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="af870-113">Přidání knihovny runtime EF Core do aplikace, nainstalujte balíček NuGet pro poskytovatele databáze, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="af870-113">To add EF Core runtime libraries to an application, install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="af870-114">Seznam podporovaných zprostředkovatelů a jejich názvy balíčků NuGet, naleznete v tématu [databáze poskytovatelé](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="af870-114">For a list of supported providers and their NuGet package names, see [Database providers](../../providers/index.md).</span></span>

<span data-ttu-id="af870-115">Pokud chcete nainstalovat nebo aktualizovat balíčky NuGet, pomocí rozhraní příkazového řádku .NET Core, Visual Studio pomocí dialogové okno Správce balíčků nebo konzole Správce balíčků Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="af870-115">To install or update NuGet packages, use the .NET Core CLI, the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

<span data-ttu-id="af870-116">Pro aplikace ASP.NET Core 2.1 v paměti a poskytovatelé serveru SQL Server budou zahrnuty automaticky, takže není nutné je instalovat samostatně.</span><span class="sxs-lookup"><span data-stu-id="af870-116">For ASP.NET Core 2.1 applications, the in-memory and SQL Server providers are automatically included, so there's no need to install them separately.</span></span>

> [!TIP]  
> <span data-ttu-id="af870-117">Pokud je potřeba aktualizovat aplikaci, která používá databázi jiného poskytovatele, vždy vyhledejte aktualizace zprostředkovatele, který je kompatibilní s verzí EF Core, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="af870-117">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="af870-118">Poskytovatelé databází pro předchozí verze například nejsou kompatibilní s verzí 2.1 runtime EF Core.</span><span class="sxs-lookup"><span data-stu-id="af870-118">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

### <a name="net-core-cli"></a><span data-ttu-id="af870-119">.NET core CLI</span><span class="sxs-lookup"><span data-stu-id="af870-119">.NET Core CLI</span></span>

<span data-ttu-id="af870-120">Následující příkaz rozhraní příkazového řádku .NET Core nainstaluje nebo aktualizuje zprostředkovatele SQL Server:</span><span class="sxs-lookup"><span data-stu-id="af870-120">The following .NET Core CLI command installs or updates the SQL Server provider:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="af870-121">Můžete určit konkrétní verzi `dotnet add package` příkaz a `-v` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="af870-121">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="af870-122">Například chcete-li nainstalovat balíčky EF Core 2.1.0, přidejte `-v 2.1.0` k příkazu.</span><span class="sxs-lookup"><span data-stu-id="af870-122">For example, to install EF Core 2.1.0 packages, append `-v 2.1.0` to the command.</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="af870-123">Dialogové okno Správce balíčků NuGet sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af870-123">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="af870-124">V nabídce vyberte **Projekt > spravovat balíčky NuGet**</span><span class="sxs-lookup"><span data-stu-id="af870-124">From the menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="af870-125">Klikněte na **Procházet** nebo **aktualizace** kartu</span><span class="sxs-lookup"><span data-stu-id="af870-125">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="af870-126">Chcete-li nainstalovat nebo aktualizovat zprostředkovatele SQL Server, vyberte `Microsoft.EntityFrameworkCore.SqlServer` balení a potvrďte.</span><span class="sxs-lookup"><span data-stu-id="af870-126">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="af870-127">Další informace najdete v tématu [dialogové okno Správce balíčků NuGet](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="af870-127">For more information, see [NuGet Package Manager Dialog](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="af870-128">Konzola správce balíčků NuGet sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af870-128">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="af870-129">V nabídce vyberte **nástroje > Správce balíčků NuGet > Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="af870-129">From the menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="af870-130">Pokud chcete nainstalovat zprostředkovatele SQL Server, spusťte následující příkaz v konzole Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="af870-130">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="af870-131">Chcete-li aktualizovat poskytovatele, použijte `Update-Package` příkazu.</span><span class="sxs-lookup"><span data-stu-id="af870-131">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="af870-132">Chcete-li zadat konkrétní verzi, použijte `-Version` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="af870-132">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="af870-133">Například chcete-li nainstalovat balíčky EF Core 2.1.0, přidejte `-Version 2.1.0` k příkazům</span><span class="sxs-lookup"><span data-stu-id="af870-133">For example, to install EF Core 2.1.0 packages, append `-Version 2.1.0` to the commands</span></span>

<span data-ttu-id="af870-134">Další informace najdete v tématu [Konzola správce balíčků](https://docs.microsoft.com/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="af870-134">For more information, see [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console).</span></span>

## <a name="get-entity-framework-core-tools"></a><span data-ttu-id="af870-135">Získat nástroje Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="af870-135">Get Entity Framework Core tools</span></span>

<span data-ttu-id="af870-136">Kromě knihovny runtime můžete nainstalovat nástroje, které můžete provádět některé úkoly související s EF Core ve vašem projektu v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="af870-136">Besides the runtime libraries, you can install tools that can perform some EF Core-related tasks in your project at design time.</span></span> <span data-ttu-id="af870-137">Můžete například vytvořit migrace, migrace použít a vytvořit model založený na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="af870-137">For example, you can create migrations, apply migrations, and create a model based on an existing database.</span></span>

<span data-ttu-id="af870-138">K dispozici jsou dvě sady nástrojů:</span><span class="sxs-lookup"><span data-stu-id="af870-138">Two sets of tools are available:</span></span>
* <span data-ttu-id="af870-139">.NET Core [nástroje rozhraní příkazového řádku (CLI)](../../miscellaneous/cli/dotnet.md) lze použít na Windows, Linux nebo macOS.</span><span class="sxs-lookup"><span data-stu-id="af870-139">The .NET Core [command-line interface (CLI) tools](../../miscellaneous/cli/dotnet.md) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="af870-140">Tyto příkazy začínají `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="af870-140">These commands begin with `dotnet ef`.</span></span> 
* <span data-ttu-id="af870-141">[Konzola správce balíčků nástroje](../../miscellaneous/cli/powershell.md) spustit ve Visual Studio 2017 pro Windows.</span><span class="sxs-lookup"><span data-stu-id="af870-141">The [Package Manager Console tools](../../miscellaneous/cli/powershell.md)  run in Visual Studio 2017 on Windows.</span></span> <span data-ttu-id="af870-142">Tyto příkazy spustit s operací, například `Add-Migration`, `Update-Database`.</span><span class="sxs-lookup"><span data-stu-id="af870-142">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="af870-143">Přestože lze použít `dotnet ef` příkazy z konzoly Správce balíčků, je vhodnější použít nástroje konzoly Správce balíčků při používání sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="af870-143">Although you can use the `dotnet ef` commands from the Package Manager Console, it's more convenient to use the Package Manager Console tools when you're using Visual Studio:</span></span>
* <span data-ttu-id="af870-144">Automaticky fungují s aktuální projekt vybraný v konzole Správce balíčků bez nutnosti ručně přepnout adresáře.</span><span class="sxs-lookup"><span data-stu-id="af870-144">They automatically work with the current project selected in the Package Manager Console without requiring manually switching directories.</span></span>  
* <span data-ttu-id="af870-145">Automaticky otevřou soubory generované záznamem pro příkazy v sadě Visual Studio po dokončení příkazu.</span><span class="sxs-lookup"><span data-stu-id="af870-145">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-cli-tools"></a><span data-ttu-id="af870-146">Získání nástrojů rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="af870-146">Get the CLI tools</span></span>

<span data-ttu-id="af870-147">`dotnet ef` Příkazy jsou součástí sady .NET Core SDK, ale chcete-li povolit příkazy budete muset nainstalovat `Microsoft.EntityFrameworkCore.Design` balíčku:</span><span class="sxs-lookup"><span data-stu-id="af870-147">The `dotnet ef` commands are included in the .NET Core SDK, but to enable the commands you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="af870-148">Pro aplikace ASP.NET Core 2.1 pro tento balíček je přiložena automaticky.</span><span class="sxs-lookup"><span data-stu-id="af870-148">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

<span data-ttu-id="af870-149">Jak je vysvětleno výše v [požadavky](#prerequisites), budete také muset nainstalovat .NET Core 2.1 SDK.</span><span class="sxs-lookup"><span data-stu-id="af870-149">As explained earlier in [Prerequisites](#prerequisites), you also need to install the .NET Core 2.1 SDK.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="af870-150">Vždy používejte verzi sady nástrojů, který odpovídá hlavní verzi balíčky runtime.</span><span class="sxs-lookup"><span data-stu-id="af870-150">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="af870-151">Získat nástroje konzoly Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="af870-151">Get the Package Manager Console tools</span></span>

<span data-ttu-id="af870-152">Chcete-li získat Konzola správce balíčků nástroje na EF Core, nainstalovat `Microsoft.EntityFrameworkCore.Tools` balíčku:</span><span class="sxs-lookup"><span data-stu-id="af870-152">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="af870-153">Pro aplikace ASP.NET Core 2.1 pro tento balíček je přiložena automaticky.</span><span class="sxs-lookup"><span data-stu-id="af870-153">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

## <a name="upgrading-to-ef-core-21"></a><span data-ttu-id="af870-154">Upgrade na EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="af870-154">Upgrading to EF Core 2.1</span></span>

<span data-ttu-id="af870-155">Pokud upgradujete existující aplikaci do EF Core 2.1, některé odkazy na balíčky starší EF Core může být potřeba odebrat ručně:</span><span class="sxs-lookup"><span data-stu-id="af870-155">If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>

* <span data-ttu-id="af870-156">Databáze jako poskytovatel návrhu balíčky `Microsoft.EntityFrameworkCore.SqlServer.Design` jsou již požadované nebo nejsou podporovány v EF Core 2.1, ale odebrána nebudou automaticky při upgradu balíčky.</span><span class="sxs-lookup"><span data-stu-id="af870-156">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but aren't automatically removed when upgrading the other packages.</span></span>

* <span data-ttu-id="af870-157">Nástroje rozhraní příkazového řádku .NET jsou teď součástí sady .NET SDK, můžete z odebrat odkaz na tento balíček *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="af870-157">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

<span data-ttu-id="af870-158">U aplikací, které se zaměřují na rozhraní .NET Framework a byly vytvořeny v dřívějších verzích sady Visual Studio Ujistěte se, že jsou kompatibilní s knihovnami .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="af870-158">For applications that target the .NET Framework and were created by earlier versions of Visual Studio, make sure that they are compatible with .NET Standard 2.0 libraries:</span></span>

  * <span data-ttu-id="af870-159">Upravte soubor projektu a ujistěte se, že ve skupině počáteční vlastnosti se zobrazí následující položky:</span><span class="sxs-lookup"><span data-stu-id="af870-159">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * <span data-ttu-id="af870-160">Projekty testů ujistěte, že je k dispozici následující položky:</span><span class="sxs-lookup"><span data-stu-id="af870-160">For test projects, also make sure the following entry is present:</span></span>

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
