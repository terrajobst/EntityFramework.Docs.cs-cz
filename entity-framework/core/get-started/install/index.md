---
title: Instalace EF Core
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 00924af2a7beaba8575e12d91678208b517f1a09
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614269"
---
# <a name="installing-ef-core"></a><span data-ttu-id="de196-102">Instalace EF Core</span><span class="sxs-lookup"><span data-stu-id="de196-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de196-103">Požadavky</span><span class="sxs-lookup"><span data-stu-id="de196-103">Prerequisites</span></span>

<span data-ttu-id="de196-104">Abyste mohli vyvíjet aplikace .NET Core 2.1 (včetně aplikací ASP.NET Core 2.1, které cílí na .NET Core) budete muset stáhnout a nainstalovat verzi [sady SDK .NET Core 2.1](https://www.microsoft.com/net/download/core) , která je vhodná pro vaši platformu.</span><span class="sxs-lookup"><span data-stu-id="de196-104">In order to develop .NET Core 2.1 applications (including ASP.NET Core 2.1 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="de196-105">**To platí i v případě, že jste si nainstalovali aplikaci Visual Studio 2017 verze 15.7.**</span><span class="sxs-lookup"><span data-stu-id="de196-105">**This is true even if you have installed Visual Studio 2017 version 15.7.**</span></span>

<span data-ttu-id="de196-106">Chcete-li použít EF Core 2.1 nebo jiná knihovna .NET Standard 2.0 na platformě .NET kromě .NET Core 2.1 (například v rozhraní .NET Framework 4.6.1 nebo vyšší) budete potřebovat verzi Nugetu, která zná .NET Standard 2.0 a jeho rozhraní kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="de196-106">In order to use EF Core 2.1 or any other .NET Standard 2.0 library with a .NET platform besides .NET Core 2.1 (for example, with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="de196-107">Tady je několik způsobů, jak lze získat:</span><span class="sxs-lookup"><span data-stu-id="de196-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="de196-108">Instalace sady Visual Studio 2017 verze 15.7</span><span class="sxs-lookup"><span data-stu-id="de196-108">Install Visual Studio 2017 version 15.7</span></span>
* <span data-ttu-id="de196-109">Pokud používáte Visual Studio 2015, [stáhnout a upgradovat na verzi 3.6.0 pro klienta NuGet](https://www.nuget.org/downloads)</span><span class="sxs-lookup"><span data-stu-id="de196-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="de196-110">Projekty vytvořené v předchozích verzích sady Visual Studio a cílí na rozhraní .NET Framework možná bude nutné další změny-li být kompatibilní s knihovnami .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="de196-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="de196-111">Upravte soubor projektu a ujistěte se, že ve skupině počáteční vlastnosti se zobrazí následující položky:</span><span class="sxs-lookup"><span data-stu-id="de196-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="de196-112">Projekty testů ujistěte, že je k dispozici následující položky:</span><span class="sxs-lookup"><span data-stu-id="de196-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="de196-113">Získávání bity</span><span class="sxs-lookup"><span data-stu-id="de196-113">Getting the bits</span></span>
<span data-ttu-id="de196-114">Doporučeným způsobem, jak do aplikace přidejte knihovny runtime EF Core je instalace poskytovatele databáze EF Core z NuGet.</span><span class="sxs-lookup"><span data-stu-id="de196-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="de196-115">Kromě knihovny runtime můžete nainstalovat nástroje, které usnadňují provést několik úloh souvisejících s EF Core ve vašem projektu v době návrhu, jako je například vytváření a použití migrace a vytváří model založený na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="de196-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="de196-116">Pokud je potřeba aktualizovat aplikaci, která používá databázi jiného poskytovatele, vždy vyhledejte aktualizace zprostředkovatele, který je kompatibilní s verzí EF Core, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="de196-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="de196-117">Poskytovatelé databází pro předchozí verze například nejsou kompatibilní s verzí 2.1 runtime EF Core.</span><span class="sxs-lookup"><span data-stu-id="de196-117">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="de196-118">Aplikace pro ASP.NET Core 2.1 můžete bez další závislosti kromě poskytovatelé databází výrobců EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="de196-118">Applications targeting ASP.NET Core 2.1 can use EF Core 2.1 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="de196-119">Aplikace cílené na předchozích verzích technologie ASP.NET Core zapotřebí provést upgrade na technologie ASP.NET Core 2.1, aby bylo možné používat EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="de196-119">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.1 in order to use EF Core 2.1.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="de196-120">Vývoj pro různé platformy pomocí rozhraní příkazového řádku .NET Core (CLI)</span><span class="sxs-lookup"><span data-stu-id="de196-120">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="de196-121">K vývoji aplikací, které se zaměřují [.NET Core](https://www.microsoft.com/net/download/core) můžete použít [ `dotnet` příkazy rozhraní příkazového řádku](https://docs.microsoft.com/dotnet/core/tools/) v kombinaci s oblíbeném textovém editoru nebo integrované vývojové prostředí (IDE) takové Visual Studio, Visual Studio for Mac nebo Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="de196-121">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac, or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="de196-122">Aplikací určených pro .NET Core vyžádat konkrétní verze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="de196-122">Applications that target .NET Core require specific versions of Visual Studio.</span></span> <span data-ttu-id="de196-123">Vývoj v .NET Core 1.x například vyžaduje Visual Studio 2017, ale vývoj pro .NET Core 2.1 vyžaduje Visual Studio 2017 verze 15.7.</span><span class="sxs-lookup"><span data-stu-id="de196-123">For example, .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.1 development requires Visual Studio 2017 version 15.7.</span></span>

<span data-ttu-id="de196-124">Pro instalaci nebo upgrade zprostředkovatele SQL Server v aplikaci .NET Core napříč platformami, přejděte do adresáře aplikace a spusťte následující příkaz v příkazovém řádku:</span><span class="sxs-lookup"><span data-stu-id="de196-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="de196-125">Můžete určit konkrétní verzi instalace v `dotnet add package` příkaz a `-v` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="de196-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="de196-126">Například chcete-li instalovat balíčky EF Core 2.1, přidejte `-v 2.1.0` k příkazu.</span><span class="sxs-lookup"><span data-stu-id="de196-126">For example, to install EF Core 2.1 packages, append `-v 2.1.0` to the command.</span></span>

<span data-ttu-id="de196-127">EF Core obsahuje sadu [další příkazy `dotnet` rozhraní příkazového řádku](../../miscellaneous/cli/dotnet.md)počínaje `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="de196-127">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="de196-128">Nástroje rozhraní příkazového řádku .NET Core pro jádro EF Core vyžadují balíček s názvem `Microsoft.EntityFrameworkCore.Design`.</span><span class="sxs-lookup"><span data-stu-id="de196-128">The .NET Core CLI tools for EF Core require a package called `Microsoft.EntityFrameworkCore.Design`.</span></span> <span data-ttu-id="de196-129">Můžete ho přidat do projektu pomocí:</span><span class="sxs-lookup"><span data-stu-id="de196-129">You can add it to the project using:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

> [!IMPORTANT]      
> <span data-ttu-id="de196-130">Vždy používejte verzi sady nástrojů, který odpovídá hlavní verzi balíčky runtime.</span><span class="sxs-lookup"><span data-stu-id="de196-130">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="de196-131">Vývoj sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de196-131">Visual Studio development</span></span>

<span data-ttu-id="de196-132">Mnoho různých typů aplikací můžete vyvíjet, které cílí na .NET Core, .NET Framework nebo jiných platformách, které EF Core pomocí sady Visual Studio podporuje.</span><span class="sxs-lookup"><span data-stu-id="de196-132">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="de196-133">V aplikaci Visual Studio můžete nainstalovat poskytovatele EF Core k databázi dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="de196-133">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="de196-134">Pomocí Nugetu [uživatelské rozhraní Správce balíčků](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span><span class="sxs-lookup"><span data-stu-id="de196-134">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="de196-135">Vyberte v nabídce **Projekt > spravovat balíčky NuGet**</span><span class="sxs-lookup"><span data-stu-id="de196-135">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="de196-136">Klikněte na **Procházet** nebo **aktualizace** kartu</span><span class="sxs-lookup"><span data-stu-id="de196-136">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="de196-137">Vyberte `Microsoft.EntityFrameworkCore.SqlServer` balíčku a požadovanou verzi a potvrzení</span><span class="sxs-lookup"><span data-stu-id="de196-137">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="de196-138">Pomocí Nugetu [Konzola správce balíčků (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span><span class="sxs-lookup"><span data-stu-id="de196-138">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="de196-139">Vyberte v nabídce **nástroje > Správce balíčků NuGet > Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="de196-139">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="de196-140">Zadejte a spuštěním následujícího příkazu v konzole PMC:</span><span class="sxs-lookup"><span data-stu-id="de196-140">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="de196-141">Můžete použít `Update-Package` místo toho příkaz k aktualizaci balíčku, který je už nainstalovaná novější verze</span><span class="sxs-lookup"><span data-stu-id="de196-141">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="de196-142">Chcete-li zadat konkrétní verzi, můžete použít `-Version` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="de196-142">To specify a specific version, you can use the `-Version` modifier.</span></span> <span data-ttu-id="de196-143">Například chcete-li instalovat balíčky EF Core 2.1, přidejte `-Version 2.1.0` k příkazům</span><span class="sxs-lookup"><span data-stu-id="de196-143">For example, to install EF Core 2.1 packages, append `-Version 2.1.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="de196-144">Nástroje</span><span class="sxs-lookup"><span data-stu-id="de196-144">Tools</span></span>

<span data-ttu-id="de196-145">Je také verze prostředí PowerShell [EF Core příkazy, které spusťte v konzole PMC](../../miscellaneous/cli/powershell.md) v sadě Visual Studio nabízí podobné funkce, `dotnet ef` příkazy.</span><span class="sxs-lookup"><span data-stu-id="de196-145">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> 

> [!TIP]  
> <span data-ttu-id="de196-146">I když je možné použít `dotnet ef` příkazy z konzole PMC v sadě Visual Studio, je mnohem více pohodlné používat ho na verzi prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="de196-146">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="de196-147">Automaticky fungují s aktuální projekt vybraný v konzole PMC bez nutnosti ručně přepnout adresáře.</span><span class="sxs-lookup"><span data-stu-id="de196-147">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="de196-148">Automaticky otevřou soubory generované záznamem pro příkazy v sadě Visual Studio po dokončení příkazu.</span><span class="sxs-lookup"><span data-stu-id="de196-148">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="de196-149">**Zastaralé balíčky v EF Core 2.1:** Pokud upgradujete existující aplikaci do EF Core 2.1, může být nutné ručně odebrat některé odkazy na balíčky starší EF Core:</span><span class="sxs-lookup"><span data-stu-id="de196-149">**Deprecated packages in EF Core 2.1:** If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>
> * <span data-ttu-id="de196-150">Databáze jako poskytovatel návrhu balíčky `Microsoft.EntityFrameworkCore.SqlServer.Design` jsou již požadované nebo nejsou podporovány v EF Core 2.1, ale nebude automaticky odebrána při upgradu balíčky.</span><span class="sxs-lookup"><span data-stu-id="de196-150">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but will not be automatically removed when upgrading the other packages.</span></span>
> * <span data-ttu-id="de196-151">Nástroje rozhraní příkazového řádku .NET jsou teď součástí sady .NET SDK, můžete z odebrat odkaz na tento balíček *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="de196-151">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>
>   ```
>   <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
>   ```
