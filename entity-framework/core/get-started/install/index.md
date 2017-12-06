---
title: "Instalace jádra EF"
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
---
# <a name="installing-ef-core"></a>Instalace jádra EF

## <a name="prerequisites"></a>Požadavky

K vývoji aplikací rozhraní .NET 2.0 jádra (včetně technologii ASP.NET 2.0 základní aplikace, které cílí na .NET Core) budete potřebovat stáhnout a nainstalovat verzi [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) , je vhodné pro vaši platformu. **To platí i v případě, že jste nainstalovali Visual Studio 2017 verze 15.3.**

Abyste mohli používat s platformy .NET kromě .NET Core 2.0 EF základní 2.0 nebo jiných rozhraní .NET 2.0 standardní knihovny (například v rozhraní .NET Framework 4.6.1 nebo vyšší) budete potřebovat verze balíčku nuget, který zná standardní rozhraní .NET 2.0 a jeho kompatibilní rozhraní. To můžete získat několika způsoby:

* Nainstalovat Visual Studio 2017 verze 15.3
* Pokud používáte Visual Studio 2015, [stáhnout a upgrade klienta NuGet verzi 3.6.0](https://www.nuget.org/downloads)

Projektů vytvořených v dřívějších verzích sady Visual Studio a cílení na rozhraní .NET Framework pravděpodobně nutné další úpravy, aby byla kompatibilní s knihovnami .NET 2.0 standardní:

* Upravte soubor projektu a ujistěte se, že se zobrazí následující položku ve skupině počáteční vlastnost:
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* Projektů testů ujistěte, že se nachází následující záznam:
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a>Získávání službu bits
Doporučeným způsobem, jak přidat základní EF runtime knihovny do aplikace je nainstalován zprostředkovatel EF základní databáze z NuGet.

Kromě modulu runtime knihoven můžete nainstalovat nástroje, které usnadňují provést některé úlohy související s EF základní ve vašem projektu v době návrhu, jako je například vytváření a použití migrace a vytvoření modelu na základě na existující databázi.

> [!TIP]  
> Pokud je potřeba aktualizovat aplikaci, která používá poskytovatele třetích stran databáze, vždycky si vyhledejte aktualizaci zprostředkovatele, který je kompatibilní s verzí EF jádra, kterou chcete použít. Například Zprostředkovatelé databáze pro předchozí verze nejsou kompatibilní s verzí 2.0 EF Core runtime.  

> [!TIP]  
> Cílení na technologii ASP.NET 2.0 základní aplikace můžete použít EF základní 2.0 bez další závislosti, kromě zprostředkovatelů databáze třetích stran. Aplikace cílené na předchozích verzích ASP.NET Core je třeba provést upgrade na technologii ASP.NET 2.0 základní Chcete-li použít EF základní 2.0.

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a>Vývoj pro různé platformy pomocí rozhraní .NET Core rozhraní příkazového řádku (CLI)

Vyvíjet aplikace, které se zaměřují [.NET Core](https://www.microsoft.com/net/download/core) můžete použít [ `dotnet` rozhraní příkazového řádku](https://docs.microsoft.com/dotnet/core/tools/) v kombinaci s svém oblíbeném textovém editoru, nebo integrované vývojové prostředí (IDE) takové jako Visual Studio, Visual Studio pro Mac nebo Visual Studio Code.

> [!IMPORTANT]  
> Aplikace, které cílí na .NET Core vyžadovat určité verze sady Visual Studio, například vývoj 1.x .NET Core vyžaduje Visual Studio 2017, zatímco .NET Core 2.0 vývoj vyžaduje Visual Studio 2017 verze 15.3.

Nainstalujte nebo upgradujte zprostředkovatele služby SQL Server v aplikaci .NET Core a platformy, přepněte do adresáře aplikace a spusťte následující příkazy v příkazovém řádku:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Můžete určit, konkrétní verzi instalaci `dotnet add package` příkaz `-v` modifikátor. Například Chcete-li instalovat balíčky EF základní 2.0, připojte `-v 2.0.0` k příkazu.

Základní EF zahrnuje sadu [další příkazy pro `dotnet` rozhraní příkazového řádku](../../miscellaneous/cli/dotnet.md), od verze `dotnet ef`. Chcete-li použít `dotnet ef` příkazy rozhraní příkazového řádku, vaše aplikace `.csproj` soubor musí obsahovat následující položku:

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

.NET Core rozhraní příkazového řádku nástroje pro základní EF také vyžadují samostatný balíček s názvem Microsoft.EntityFrameworkCore.Design. Můžete je jednoduše přidat do projektu pomocí:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> Vždy používejte verzi nástroje pro balíčky, které odpovídají hlavní verzi modulu runtime balíčky.

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a>Vývoj pro Visual Studio

Mnoho různých typů aplikací můžete vyvíjet, že cíl .NET Core, rozhraní .NET Framework nebo jiných platformách podporovaných aplikací jádra EF pomocí sady Visual Studio.

V aplikaci ze sady Visual Studio můžete nainstalovat poskytovatele databáze EF základní dvěma způsoby:

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a>Pomocí NuGet [uživatelské rozhraní Správce balíčků](https://docs.microsoft.com/nuget/tools/package-manager-ui)

* Vyberte v nabídce **Projekt > Správa balíčků NuGet**

* Klikněte na **Procházet** nebo **aktualizace** karta

* Vyberte `Microsoft.EntityFrameworkCore.SqlServer` balíčku a požadovanou verzi a potvrďte

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a>Pomocí NuGet [Konzola správce balíčků (pomocí PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)

* Vyberte v nabídce **nástroje > Správce balíčků NuGet > Konzola správce balíčků**

* Zadejte a spusťte následující příkaz v pomocí PMC:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Můžete použít `Update-Package` příkaz místo toho pokud chcete aktualizovat balíček, který už je nainstalovaný na novější verzi

* Chcete-li zadat konkrétní verzi, můžete použít `-Version` modifikátor, například chcete-li instalovat balíčky EF základní 2.0, připojte `-Version 2.0.0` k příkazům

#### <a name="tools"></a>Nástroje

Je také verze prostředí PowerShell [EF základní příkazy, které spustit uvnitř pomocí PMC](../../miscellaneous/cli/powershell.md) v sadě Visual Studio, s podobné funkce, které `dotnet ef` příkazy. Chcete-li použít tyto, nainstalujte `Microsoft.EntityFrameworkCore.Tools` balíček pomocí uživatelského rozhraní Správce balíčků nebo pomocí PMC.

> [!IMPORTANT]  
> Vždy používejte verzi nástroje pro balíčky, které odpovídají hlavní verzi modulu runtime balíčky.

> [!TIP]  
> I když je možné použít `dotnet ef` příkazy z pomocí PMC v sadě Visual Studio je mnohem pohodlnější použití verze prostředí PowerShell:
> * Automaticky pracují s aktuální projekt vybraný v pomocí PMC bez nutnosti ručně přepínání adresáře.  
> * Otevření automaticky soubory generované příkazy v sadě Visual Studio po dokončení příkazu.

> [!IMPORTANT]  
> **Zastaralé balíčky v EF základní 2.0:** Pokud upgradujete stávající aplikace do EF základní 2.0, některé odkazy na balíčky starší EF základní může třeba je odebrat ručně. Zejména, například databáze zprostředkovatele návrhu balíčky `Microsoft.EntityFrameworkCore.SqlServer.Design` jsou již požadované nebo nejsou podporovány v EF základní 2.0, ale nebude automaticky odebrána při upgradu ostatní balíčky.
