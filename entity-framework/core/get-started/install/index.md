---
title: Instalace EF Core
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 7bb2ee11940a4fd5736c7a23c16533ef53018f7b
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949189"
---
# <a name="installing-ef-core"></a><span data-ttu-id="15727-102">Instalace EF Core</span><span class="sxs-lookup"><span data-stu-id="15727-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15727-103">Požadavky</span><span class="sxs-lookup"><span data-stu-id="15727-103">Prerequisites</span></span>

<span data-ttu-id="15727-104">Abyste mohli vyvíjet aplikace .NET Core 2.0 (včetně aplikací ASP.NET Core 2.0, které cílit na .NET Core) budete muset stáhnout a nainstalovat verzi [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) , která je vhodná pro vaši platformu.</span><span class="sxs-lookup"><span data-stu-id="15727-104">In order to develop .NET Core 2.0 applications (including ASP.NET Core 2.0 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="15727-105">**To platí i v případě, že jste si nainstalovali aplikaci Visual Studio 2017 verze 15.3.**</span><span class="sxs-lookup"><span data-stu-id="15727-105">**This is true even if you have installed Visual Studio 2017 version 15.3.**</span></span>

<span data-ttu-id="15727-106">Chcete-li použít EF Core 2.0 nebo jiná knihovna .NET Standard 2.0 pomocí platformy .NET kromě .NET Core 2.0 (například v rozhraní .NET Framework 4.6.1 nebo vyšší) budete potřebovat verzi Nugetu, která zná .NET Standard 2.0 a jeho rozhraní kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="15727-106">In order to use EF Core 2.0 or any other .NET Standard 2.0 library with a .NET platforms besides .NET Core 2.0 (for example, with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="15727-107">Tady je několik způsobů, jak lze získat:</span><span class="sxs-lookup"><span data-stu-id="15727-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="15727-108">Instalace sady Visual Studio 2017 verze 15.3</span><span class="sxs-lookup"><span data-stu-id="15727-108">Install Visual Studio 2017 version 15.3</span></span>
* <span data-ttu-id="15727-109">Pokud používáte Visual Studio 2015, [stáhnout a upgradovat na verzi 3.6.0 pro klienta NuGet](https://www.nuget.org/downloads)</span><span class="sxs-lookup"><span data-stu-id="15727-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="15727-110">Projekty vytvořené v předchozích verzích sady Visual Studio a cílí na rozhraní .NET Framework možná bude nutné další změny-li být kompatibilní s knihovnami .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="15727-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="15727-111">Upravte soubor projektu a ujistěte se, že ve skupině počáteční vlastnosti se zobrazí následující položky:</span><span class="sxs-lookup"><span data-stu-id="15727-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="15727-112">Projekty testů ujistěte, že je k dispozici následující položky:</span><span class="sxs-lookup"><span data-stu-id="15727-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="15727-113">Získávání bity</span><span class="sxs-lookup"><span data-stu-id="15727-113">Getting the bits</span></span>
<span data-ttu-id="15727-114">Doporučeným způsobem, jak do aplikace přidejte knihovny runtime EF Core je instalace poskytovatele databáze EF Core z NuGet.</span><span class="sxs-lookup"><span data-stu-id="15727-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="15727-115">Kromě knihovny runtime můžete nainstalovat nástroje, které usnadňují provést několik úloh souvisejících s EF Core ve vašem projektu v době návrhu, jako je například vytváření a použití migrace a vytváří model založený na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="15727-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="15727-116">Pokud je potřeba aktualizovat aplikaci, která používá databázi jiného poskytovatele, vždy vyhledejte aktualizace zprostředkovatele, který je kompatibilní s verzí EF Core, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="15727-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="15727-117">Poskytovatelé databází pro předchozí verze například nejsou kompatibilní s verzí modulu runtime EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="15727-117">For example, database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="15727-118">Aplikace pro ASP.NET Core 2.0, můžete použít EF Core 2.0 bez další závislosti kromě poskytovatelé databází výrobců.</span><span class="sxs-lookup"><span data-stu-id="15727-118">Applications targeting ASP.NET Core 2.0 can use EF Core 2.0 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="15727-119">Aplikace cílené na předchozích verzích technologie ASP.NET Core zapotřebí provést upgrade na technologie ASP.NET Core 2.0, aby bylo možné používat EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="15727-119">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.0 in order to use EF Core 2.0.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="15727-120">Vývoj pro různé platformy pomocí rozhraní příkazového řádku .NET Core (CLI)</span><span class="sxs-lookup"><span data-stu-id="15727-120">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="15727-121">K vývoji aplikací, které se zaměřují [.NET Core](https://www.microsoft.com/net/download/core) můžete použít [ `dotnet` příkazy rozhraní příkazového řádku](https://docs.microsoft.com/dotnet/core/tools/) v kombinaci s oblíbeném textovém editoru nebo integrované vývojové prostředí (IDE) takové Visual Studio, Visual Studio for Mac nebo Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="15727-121">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="15727-122">Aplikací určených pro .NET Core vyžádat konkrétní verze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15727-122">Applications that target .NET Core require specific versions of Visual Studio.</span></span> <span data-ttu-id="15727-123">Vývoj v .NET Core 1.x například vyžaduje Visual Studio 2017, ale vývoj pro .NET Core 2.0 vyžaduje Visual Studio 2017 verze 15.3.</span><span class="sxs-lookup"><span data-stu-id="15727-123">For example, .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.0 development requires Visual Studio 2017 version 15.3.</span></span>

<span data-ttu-id="15727-124">Pro instalaci nebo upgrade zprostředkovatele SQL Server v aplikaci .NET Core napříč platformami, přejděte do adresáře aplikace a spusťte následující příkaz v příkazovém řádku:</span><span class="sxs-lookup"><span data-stu-id="15727-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="15727-125">Můžete určit konkrétní verzi instalace v `dotnet add package` příkaz a `-v` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="15727-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="15727-126">Například chcete-li instalovat balíčky EF Core 2.0, přidejte `-v 2.0.0` k příkazu.</span><span class="sxs-lookup"><span data-stu-id="15727-126">For example, to install EF Core 2.0 packages, append `-v 2.0.0` to the command.</span></span>

<span data-ttu-id="15727-127">EF Core obsahuje sadu [další příkazy `dotnet` rozhraní příkazového řádku](../../miscellaneous/cli/dotnet.md)počínaje `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="15727-127">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="15727-128">Chcete-li použít `dotnet ef` příkazy rozhraní příkazového řádku, vaše aplikace `.csproj` soubor musí obsahovat následující položku:</span><span class="sxs-lookup"><span data-stu-id="15727-128">In order to use the `dotnet ef` CLI commands, your application’s `.csproj` file needs to contain the following entry:</span></span>

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="15727-129">Nástroje rozhraní příkazového řádku .NET Core pro jádro EF Core také vyžadovat samostatný balíček s názvem Microsoft.EntityFrameworkCore.Design.</span><span class="sxs-lookup"><span data-stu-id="15727-129">The .NET Core CLI tools for EF Core also require a separate package called Microsoft.EntityFrameworkCore.Design.</span></span> <span data-ttu-id="15727-130">Můžete ji jednoduše přidat do projektu pomocí:</span><span class="sxs-lookup"><span data-stu-id="15727-130">You can simply add it to the project using:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> <span data-ttu-id="15727-131">Vždy používejte balíčky nástrojů, které odpovídají hlavní verzi balíčky runtime verze.</span><span class="sxs-lookup"><span data-stu-id="15727-131">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="15727-132">Vývoj sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15727-132">Visual Studio development</span></span>

<span data-ttu-id="15727-133">Mnoho různých typů aplikací můžete vyvíjet, které cílí na .NET Core, .NET Framework nebo jiných platformách, které EF Core pomocí sady Visual Studio podporuje.</span><span class="sxs-lookup"><span data-stu-id="15727-133">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="15727-134">V aplikaci Visual Studio můžete nainstalovat poskytovatele EF Core k databázi dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="15727-134">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="15727-135">Pomocí Nugetu [uživatelské rozhraní Správce balíčků](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span><span class="sxs-lookup"><span data-stu-id="15727-135">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="15727-136">Vyberte v nabídce **Projekt > spravovat balíčky NuGet**</span><span class="sxs-lookup"><span data-stu-id="15727-136">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="15727-137">Klikněte na **Procházet** nebo **aktualizace** kartu</span><span class="sxs-lookup"><span data-stu-id="15727-137">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="15727-138">Vyberte `Microsoft.EntityFrameworkCore.SqlServer` balíčku a požadovanou verzi a potvrzení</span><span class="sxs-lookup"><span data-stu-id="15727-138">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="15727-139">Pomocí Nugetu [Konzola správce balíčků (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span><span class="sxs-lookup"><span data-stu-id="15727-139">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="15727-140">Vyberte v nabídce **nástroje > Správce balíčků NuGet > Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="15727-140">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="15727-141">Zadejte a spuštěním následujícího příkazu v konzole PMC:</span><span class="sxs-lookup"><span data-stu-id="15727-141">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="15727-142">Můžete použít `Update-Package` místo toho příkaz k aktualizaci balíčku, který je už nainstalovaná novější verze</span><span class="sxs-lookup"><span data-stu-id="15727-142">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="15727-143">Chcete-li zadat konkrétní verzi, můžete použít `-Version` modifikátor.</span><span class="sxs-lookup"><span data-stu-id="15727-143">To specify a specific version, you can use the `-Version` modifier.</span></span> <span data-ttu-id="15727-144">Například chcete-li instalovat balíčky EF Core 2.0, přidejte `-Version 2.0.0` k příkazům</span><span class="sxs-lookup"><span data-stu-id="15727-144">For example, to install EF Core 2.0 packages, append `-Version 2.0.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="15727-145">Nástroje</span><span class="sxs-lookup"><span data-stu-id="15727-145">Tools</span></span>

<span data-ttu-id="15727-146">Je také verze prostředí PowerShell [EF Core příkazy, které spusťte v konzole PMC](../../miscellaneous/cli/powershell.md) v sadě Visual Studio nabízí podobné funkce, `dotnet ef` příkazy.</span><span class="sxs-lookup"><span data-stu-id="15727-146">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> <span data-ttu-id="15727-147">Pokud chcete použít, nainstalujte `Microsoft.EntityFrameworkCore.Tools` balíček pomocí uživatelského rozhraní Správce balíčků nebo konzolu PMC.</span><span class="sxs-lookup"><span data-stu-id="15727-147">In order to use these, install the `Microsoft.EntityFrameworkCore.Tools` package using either the Package Manager UI or the PMC.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="15727-148">Vždy používejte balíčky nástrojů, které odpovídají hlavní verzi balíčky runtime verze.</span><span class="sxs-lookup"><span data-stu-id="15727-148">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

> [!TIP]  
> <span data-ttu-id="15727-149">I když je možné použít `dotnet ef` příkazy z konzole PMC v sadě Visual Studio, je mnohem více pohodlné používat ho na verzi prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="15727-149">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="15727-150">Automaticky fungují s aktuální projekt vybraný v konzole PMC bez nutnosti ručně přepnout adresáře.</span><span class="sxs-lookup"><span data-stu-id="15727-150">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="15727-151">Automaticky otevřou soubory generované záznamem pro příkazy v sadě Visual Studio po dokončení příkazu.</span><span class="sxs-lookup"><span data-stu-id="15727-151">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="15727-152">**Zastaralé balíčky v EF Core 2.0:** Pokud provádíte upgrade stávající aplikace na EF Core 2.0, některé odkazy na balíčky starší EF Core může být nutné ručně odebrat.</span><span class="sxs-lookup"><span data-stu-id="15727-152">**Deprecated packages in EF Core 2.0:** If you are upgrading an existing application to EF Core 2.0, some references to older EF Core packages may need to be removed manually.</span></span> <span data-ttu-id="15727-153">Zejména databáze jako poskytovatel návrhu balíčky `Microsoft.EntityFrameworkCore.SqlServer.Design` jsou už povinné nebo nejsou podporovány v EF Core 2.0, ale nebude automaticky odebrána při upgradu balíčky.</span><span class="sxs-lookup"><span data-stu-id="15727-153">In particular, database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.0, but will not be automatically removed when upgrading the other packages.</span></span>
