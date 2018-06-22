---
title: Instalace jádra EF
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 31b96ebd0ae282b88be98988eff6263084dc5dd5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
ms.locfileid: "26054646"
---
# <a name="installing-ef-core"></a><span data-ttu-id="43ff6-102">Instalace jádra EF</span><span class="sxs-lookup"><span data-stu-id="43ff6-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43ff6-103">Požadavky</span><span class="sxs-lookup"><span data-stu-id="43ff6-103">Prerequisites</span></span>

<span data-ttu-id="43ff6-104">K vývoji aplikací rozhraní .NET 2.0 jádra (včetně technologii ASP.NET 2.0 základní aplikace, které cílí na .NET Core) budete potřebovat stáhnout a nainstalovat verzi [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) , je vhodné pro vaši platformu.</span><span class="sxs-lookup"><span data-stu-id="43ff6-104">In order to develop .NET Core 2.0 applications (including ASP.NET Core 2.0 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="43ff6-105">**To platí i v případě, že jste nainstalovali Visual Studio 2017 verze 15.3.**</span><span class="sxs-lookup"><span data-stu-id="43ff6-105">**This is true even if you have installed Visual Studio 2017 version 15.3.**</span></span>

<span data-ttu-id="43ff6-106">Abyste mohli používat s platformy .NET kromě .NET Core 2.0 EF základní 2.0 nebo jiných rozhraní .NET 2.0 standardní knihovny (například v rozhraní .NET Framework 4.6.1 nebo vyšší) budete potřebovat verze balíčku nuget, který zná standardní rozhraní .NET 2.0 a jeho kompatibilní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="43ff6-106">In order to use EF Core 2.0 or any other .NET Standard 2.0 library with a .NET platforms besides .NET Core 2.0 (e.g. with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="43ff6-107">To můžete získat několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="43ff6-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="43ff6-108">Nainstalovat Visual Studio 2017 verze 15.3</span><span class="sxs-lookup"><span data-stu-id="43ff6-108">Install Visual Studio 2017 version 15.3</span></span>
* <span data-ttu-id="43ff6-109">Pokud používáte Visual Studio 2015, [stáhnout a upgrade klienta NuGet verzi 3.6.0](https://www.nuget.org/downloads)</span><span class="sxs-lookup"><span data-stu-id="43ff6-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="43ff6-110">Projektů vytvořených v dřívějších verzích sady Visual Studio a cílení na rozhraní .NET Framework pravděpodobně nutné další úpravy, aby byla kompatibilní s knihovnami .NET 2.0 standardní:</span><span class="sxs-lookup"><span data-stu-id="43ff6-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="43ff6-111">Upravte soubor projektu a ujistěte se, že se zobrazí následující položku ve skupině počáteční vlastnost:</span><span class="sxs-lookup"><span data-stu-id="43ff6-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="43ff6-112">Projektů testů ujistěte, že se nachází následující záznam:</span><span class="sxs-lookup"><span data-stu-id="43ff6-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="43ff6-113">Získávání službu bits</span><span class="sxs-lookup"><span data-stu-id="43ff6-113">Getting the bits</span></span>
<span data-ttu-id="43ff6-114">Doporučeným způsobem, jak přidat základní EF runtime knihovny do aplikace je nainstalován zprostředkovatel EF základní databáze z NuGet.</span><span class="sxs-lookup"><span data-stu-id="43ff6-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="43ff6-115">Kromě modulu runtime knihoven můžete nainstalovat nástroje, které usnadňují provést některé úlohy související s EF základní ve vašem projektu v době návrhu, jako je například vytváření a použití migrace a vytvoření modelu na základě na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="43ff6-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="43ff6-116">Pokud je potřeba aktualizovat aplikaci, která používá poskytovatele třetích stran databáze, vždycky si vyhledejte aktualizaci zprostředkovatele, který je kompatibilní s verzí EF jádra, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="43ff6-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="43ff6-117">Například</span><span class="sxs-lookup"><span data-stu-id="43ff6-117">E.g.</span></span> <span data-ttu-id="43ff6-118">Zprostředkovatelé databáze pro předchozí verze nejsou kompatibilní s verzí 2.0 EF Core runtime.</span><span class="sxs-lookup"><span data-stu-id="43ff6-118">database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="43ff6-119">Cílení na technologii ASP.NET 2.0 základní aplikace můžete použít EF základní 2.0 bez další závislosti, kromě zprostředkovatelů databáze třetích stran.</span><span class="sxs-lookup"><span data-stu-id="43ff6-119">Applications targeting ASP.NET Core 2.0 can use EF Core 2.0 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="43ff6-120">Aplikace cílené na předchozích verzích ASP.NET Core je třeba provést upgrade na technologii ASP.NET 2.0 základní Chcete-li použít EF základní 2.0.</span><span class="sxs-lookup"><span data-stu-id="43ff6-120">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.0 in order to use EF Core 2.0.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="43ff6-121">Vývoj pro různé platformy pomocí rozhraní .NET Core rozhraní příkazového řádku (CLI)</span><span class="sxs-lookup"><span data-stu-id="43ff6-121">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="43ff6-122">Vyvíjet aplikace, které se zaměřují [.NET Core](https://www.microsoft.com/net/download/core) můžete použít [ `dotnet` rozhraní příkazového řádku](https://docs.microsoft.com/dotnet/core/tools/) v kombinaci s svém oblíbeném textovém editoru, nebo integrované vývojové prostředí (IDE) takové jako Visual Studio, Visual Studio pro Mac nebo Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="43ff6-122">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="43ff6-123">Aplikace, které cílí na .NET Core vyžadovat určité verze sady Visual Studio, například vývoj 1.x .NET Core vyžaduje Visual Studio 2017, zatímco .NET Core 2.0 vývoj vyžaduje Visual Studio 2017 verze 15.3.</span><span class="sxs-lookup"><span data-stu-id="43ff6-123">Applications that target .NET Core require specific versions of Visual Studio, e.g. .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.0 development requires Visual Studio 2017 version 15.3.</span></span>

<span data-ttu-id="43ff6-124">Nainstalujte nebo upgradujte zprostředkovatele služby SQL Server v aplikaci .NET Core a platformy, přepněte do adresáře aplikace a spusťte následující příkazy v příkazovém řádku:</span><span class="sxs-lookup"><span data-stu-id="43ff6-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="43ff6-125">Můžete určit, konkrétní verzi instalaci `dotnet add package` příkaz `-v` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="43ff6-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="43ff6-126">Například</span><span class="sxs-lookup"><span data-stu-id="43ff6-126">E.g.</span></span> <span data-ttu-id="43ff6-127">Chcete-li instalovat balíčky EF základní 2.0, připojte `-v 2.0.0` k příkazu.</span><span class="sxs-lookup"><span data-stu-id="43ff6-127">to install EF Core 2.0 packages, append `-v 2.0.0` to the command.</span></span>

<span data-ttu-id="43ff6-128">Základní EF zahrnuje sadu [další příkazy pro `dotnet` rozhraní příkazového řádku](../../miscellaneous/cli/dotnet.md), od verze `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="43ff6-128">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="43ff6-129">Chcete-li použít `dotnet ef` příkazy rozhraní příkazového řádku, vaše aplikace `.csproj` soubor musí obsahovat následující položku:</span><span class="sxs-lookup"><span data-stu-id="43ff6-129">In order to use the `dotnet ef` CLI commands, your application’s `.csproj` file needs to contain the following entry:</span></span>

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="43ff6-130">.NET Core rozhraní příkazového řádku nástroje pro základní EF také vyžadují samostatný balíček s názvem Microsoft.EntityFrameworkCore.Design.</span><span class="sxs-lookup"><span data-stu-id="43ff6-130">The .NET Core CLI tools for EF Core also require a separate package called Microsoft.EntityFrameworkCore.Design.</span></span> <span data-ttu-id="43ff6-131">Můžete je jednoduše přidat do projektu pomocí:</span><span class="sxs-lookup"><span data-stu-id="43ff6-131">You can simply add it to the project using:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> <span data-ttu-id="43ff6-132">Vždy používejte verzi nástroje pro balíčky, které odpovídají hlavní verzi modulu runtime balíčky.</span><span class="sxs-lookup"><span data-stu-id="43ff6-132">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="43ff6-133">Vývoj pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43ff6-133">Visual Studio development</span></span>

<span data-ttu-id="43ff6-134">Mnoho různých typů aplikací můžete vyvíjet, že cíl .NET Core, rozhraní .NET Framework nebo jiných platformách podporovaných aplikací jádra EF pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="43ff6-134">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="43ff6-135">V aplikaci ze sady Visual Studio můžete nainstalovat poskytovatele databáze EF základní dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="43ff6-135">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="43ff6-136">Pomocí NuGet [uživatelské rozhraní Správce balíčků](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span><span class="sxs-lookup"><span data-stu-id="43ff6-136">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="43ff6-137">Vyberte v nabídce **Projekt > Správa balíčků NuGet**</span><span class="sxs-lookup"><span data-stu-id="43ff6-137">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="43ff6-138">Klikněte na **Procházet** nebo **aktualizace** karta</span><span class="sxs-lookup"><span data-stu-id="43ff6-138">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="43ff6-139">Vyberte `Microsoft.EntityFrameworkCore.SqlServer` balíčku a požadovanou verzi a potvrďte</span><span class="sxs-lookup"><span data-stu-id="43ff6-139">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="43ff6-140">Pomocí NuGet [Konzola správce balíčků (pomocí PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span><span class="sxs-lookup"><span data-stu-id="43ff6-140">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="43ff6-141">Vyberte v nabídce **nástroje > Správce balíčků NuGet > Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="43ff6-141">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="43ff6-142">Zadejte a spusťte následující příkaz v pomocí PMC:</span><span class="sxs-lookup"><span data-stu-id="43ff6-142">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="43ff6-143">Můžete použít `Update-Package` příkaz místo toho pokud chcete aktualizovat balíček, který už je nainstalovaný na novější verzi</span><span class="sxs-lookup"><span data-stu-id="43ff6-143">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="43ff6-144">Chcete-li zadat konkrétní verzi, můžete použít `-Version` modifikátor, například chcete-li instalovat balíčky EF základní 2.0, připojte `-Version 2.0.0` k příkazům</span><span class="sxs-lookup"><span data-stu-id="43ff6-144">To specify a specific version, you can use the `-Version` modifier, e.g. to install EF Core 2.0 packages, append `-Version 2.0.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="43ff6-145">Nástroje</span><span class="sxs-lookup"><span data-stu-id="43ff6-145">Tools</span></span>

<span data-ttu-id="43ff6-146">Je také verze prostředí PowerShell [EF základní příkazy, které spustit uvnitř pomocí PMC](../../miscellaneous/cli/powershell.md) v sadě Visual Studio, s podobné funkce, které `dotnet ef` příkazy.</span><span class="sxs-lookup"><span data-stu-id="43ff6-146">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> <span data-ttu-id="43ff6-147">Chcete-li použít tyto, nainstalujte `Microsoft.EntityFrameworkCore.Tools` balíček pomocí uživatelského rozhraní Správce balíčků nebo pomocí PMC.</span><span class="sxs-lookup"><span data-stu-id="43ff6-147">In order to use these, install the `Microsoft.EntityFrameworkCore.Tools` package using either the Package Manager UI or the PMC.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="43ff6-148">Vždy používejte verzi nástroje pro balíčky, které odpovídají hlavní verzi modulu runtime balíčky.</span><span class="sxs-lookup"><span data-stu-id="43ff6-148">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

> [!TIP]  
> <span data-ttu-id="43ff6-149">I když je možné použít `dotnet ef` příkazy z pomocí PMC v sadě Visual Studio je mnohem pohodlnější použití verze prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="43ff6-149">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="43ff6-150">Automaticky pracují s aktuální projekt vybraný v pomocí PMC bez nutnosti ručně přepínání adresáře.</span><span class="sxs-lookup"><span data-stu-id="43ff6-150">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="43ff6-151">Otevření automaticky soubory generované příkazy v sadě Visual Studio po dokončení příkazu.</span><span class="sxs-lookup"><span data-stu-id="43ff6-151">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="43ff6-152">**Zastaralé balíčky v EF základní 2.0:** Pokud upgradujete stávající aplikace do EF základní 2.0, některé odkazy na balíčky starší EF základní může třeba je odebrat ručně.</span><span class="sxs-lookup"><span data-stu-id="43ff6-152">**Deprecated packages in EF Core 2.0:** If you are upgrading an existing application to EF Core 2.0, some references to older EF Core packages may need to be removed manually.</span></span> <span data-ttu-id="43ff6-153">Zejména, například databáze zprostředkovatele návrhu balíčky `Microsoft.EntityFrameworkCore.SqlServer.Design` jsou již požadované nebo nejsou podporovány v EF základní 2.0, ale nebude automaticky odebrána při upgradu ostatní balíčky.</span><span class="sxs-lookup"><span data-stu-id="43ff6-153">In particular, database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.0, but will not be automatically removed when upgrading the other packages.</span></span>
