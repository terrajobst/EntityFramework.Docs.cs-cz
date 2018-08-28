---
title: Instalace EF Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 30ca81a0ede65506a6684d2322d31332115b1ed3
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996925"
---
# <a name="installing-ef-core"></a>Instalace EF Core

## <a name="prerequisites"></a>Požadavky

Abyste mohli vyvíjet aplikace .NET Core 2.1 (včetně aplikací ASP.NET Core 2.1, které cílí na .NET Core) budete muset stáhnout a nainstalovat verzi [sady SDK .NET Core 2.1](https://www.microsoft.com/net/download/core) , která je vhodná pro vaši platformu. **To platí i v případě, že jste si nainstalovali aplikaci Visual Studio 2017 verze 15.7.**

Chcete-li použít EF Core 2.1 nebo jiná knihovna .NET Standard 2.0 na platformě .NET kromě .NET Core 2.1 (například v rozhraní .NET Framework 4.6.1 nebo vyšší) budete potřebovat verzi Nugetu, která zná .NET Standard 2.0 a jeho rozhraní kompatibilní. Tady je několik způsobů, jak lze získat:

* Instalace sady Visual Studio 2017 verze 15.7
* Pokud používáte Visual Studio 2015, [stáhnout a upgradovat na verzi 3.6.0 pro klienta NuGet](https://www.nuget.org/downloads)

Projekty vytvořené v předchozích verzích sady Visual Studio a cílí na rozhraní .NET Framework možná bude nutné další změny-li být kompatibilní s knihovnami .NET Standard 2.0:

* Upravte soubor projektu a ujistěte se, že ve skupině počáteční vlastnosti se zobrazí následující položky:
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* Projekty testů ujistěte, že je k dispozici následující položky:
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a>Získávání bity
Doporučeným způsobem, jak do aplikace přidejte knihovny runtime EF Core je instalace poskytovatele databáze EF Core z NuGet.

Kromě knihovny runtime můžete nainstalovat nástroje, které usnadňují provést několik úloh souvisejících s EF Core ve vašem projektu v době návrhu, jako je například vytváření a použití migrace a vytváří model založený na existující databázi.

> [!TIP]  
> Pokud je potřeba aktualizovat aplikaci, která používá databázi jiného poskytovatele, vždy vyhledejte aktualizace zprostředkovatele, který je kompatibilní s verzí EF Core, kterou chcete použít. Poskytovatelé databází pro předchozí verze například nejsou kompatibilní s verzí 2.1 runtime EF Core.  

> [!TIP]  
> Aplikace pro ASP.NET Core 2.1 můžete bez další závislosti kromě poskytovatelé databází výrobců EF Core 2.1. Aplikace cílené na předchozích verzích technologie ASP.NET Core zapotřebí provést upgrade na technologie ASP.NET Core 2.1, aby bylo možné používat EF Core 2.1.

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a>Vývoj pro různé platformy pomocí rozhraní příkazového řádku .NET Core (CLI)

K vývoji aplikací, které se zaměřují [.NET Core](https://www.microsoft.com/net/download/core) můžete použít [ `dotnet` příkazy rozhraní příkazového řádku](https://docs.microsoft.com/dotnet/core/tools/) v kombinaci s oblíbeném textovém editoru nebo integrované vývojové prostředí (IDE) takové Visual Studio, Visual Studio for Mac nebo Visual Studio Code.

> [!IMPORTANT]  
> Aplikací určených pro .NET Core vyžádat konkrétní verze sady Visual Studio. Vývoj v .NET Core 1.x například vyžaduje Visual Studio 2017, ale vývoj pro .NET Core 2.1 vyžaduje Visual Studio 2017 verze 15.7.

Pro instalaci nebo upgrade zprostředkovatele SQL Server v aplikaci .NET Core napříč platformami, přejděte do adresáře aplikace a spusťte následující příkaz v příkazovém řádku:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Můžete určit konkrétní verzi instalace v `dotnet add package` příkaz a `-v` modifikátor. Například chcete-li instalovat balíčky EF Core 2.1, přidejte `-v 2.1.0` k příkazu.

EF Core obsahuje sadu [další příkazy `dotnet` rozhraní příkazového řádku](../../miscellaneous/cli/dotnet.md)počínaje `dotnet ef`. Nástroje rozhraní příkazového řádku .NET Core pro jádro EF Core vyžadují balíček s názvem `Microsoft.EntityFrameworkCore.Design`. Můžete ho přidat do projektu pomocí:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

> [!IMPORTANT]      
> Vždy používejte verzi sady nástrojů, který odpovídá hlavní verzi balíčky runtime.

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a>Vývoj sady Visual Studio

Mnoho různých typů aplikací můžete vyvíjet, které cílí na .NET Core, .NET Framework nebo jiných platformách, které EF Core pomocí sady Visual Studio podporuje.

V aplikaci Visual Studio můžete nainstalovat poskytovatele EF Core k databázi dvěma způsoby:

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a>Pomocí Nugetu [uživatelské rozhraní Správce balíčků](https://docs.microsoft.com/nuget/tools/package-manager-ui)

* Vyberte v nabídce **Projekt > spravovat balíčky NuGet**

* Klikněte na **Procházet** nebo **aktualizace** kartu

* Vyberte `Microsoft.EntityFrameworkCore.SqlServer` balíčku a požadovanou verzi a potvrzení

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a>Pomocí Nugetu [Konzola správce balíčků (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)

* Vyberte v nabídce **nástroje > Správce balíčků NuGet > Konzola správce balíčků**

* Zadejte a spuštěním následujícího příkazu v konzole PMC:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Můžete použít `Update-Package` místo toho příkaz k aktualizaci balíčku, který je už nainstalovaná novější verze

* Chcete-li zadat konkrétní verzi, můžete použít `-Version` modifikátor. Například chcete-li instalovat balíčky EF Core 2.1, přidejte `-Version 2.1.0` k příkazům

#### <a name="tools"></a>Nástroje

Je také verze prostředí PowerShell [EF Core příkazy, které spusťte v konzole PMC](../../miscellaneous/cli/powershell.md) v sadě Visual Studio nabízí podobné funkce, `dotnet ef` příkazy. 

> [!TIP]  
> I když je možné použít `dotnet ef` příkazy z konzole PMC v sadě Visual Studio, je mnohem více pohodlné používat ho na verzi prostředí PowerShell:
> * Automaticky fungují s aktuální projekt vybraný v konzole PMC bez nutnosti ručně přepnout adresáře.  
> * Automaticky otevřou soubory generované záznamem pro příkazy v sadě Visual Studio po dokončení příkazu.

> [!IMPORTANT]  
> **Zastaralé balíčky v EF Core 2.1:** Pokud upgradujete existující aplikaci do EF Core 2.1, může být nutné ručně odebrat některé odkazy na balíčky starší EF Core:
> * Databáze jako poskytovatel návrhu balíčky `Microsoft.EntityFrameworkCore.SqlServer.Design` jsou již požadované nebo nejsou podporovány v EF Core 2.1, ale nebude automaticky odebrána při upgradu balíčky.
> * Nástroje rozhraní příkazového řádku .NET jsou teď součástí sady .NET SDK, můžete z odebrat odkaz na tento balíček *.csproj* souboru:
>   ```
>   <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
>   ```
